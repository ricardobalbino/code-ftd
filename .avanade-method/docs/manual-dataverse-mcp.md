# Manual Completo: Dataverse MCP Server (mwhesse)

**Versão**: 1.0 | **Data**: 2026-03-16
**Projeto**: FTD Educação — Avanade Method
**Repositório**: https://github.com/mwhesse/dataverse-mcp (v0.2.8, MIT License)

---

## Índice

1. [O que é e para que serve](#1-o-que-é-e-para-que-serve)
2. [Pré-requisitos](#2-pré-requisitos)
3. [FASE 1: Azure App Registration](#3-fase-1-azure-app-registration)
4. [FASE 2: Application User no Dataverse](#4-fase-2-application-user-no-dataverse)
5. [FASE 3: Instalação Local](#5-fase-3-instalação-local)
6. [FASE 4: Configuração no VS Code](#6-fase-4-configuração-no-vs-code)
7. [FASE 5: Validação da Instalação](#7-fase-5-validação-da-instalação)
8. [Catálogo Completo de Ferramentas (40+)](#8-catálogo-completo-de-ferramentas)
9. [Workflows de Uso Prático](#9-workflows-de-uso-prático)
10. [Configuração de Segurança (alwaysAllow)](#10-configuração-de-segurança-alwaysallow)
11. [Troubleshooting](#11-troubleshooting)
12. [Boas Práticas](#12-boas-práticas)
13. [Integração com Avanade Method / BMAD](#13-integração-com-avanade-method)

---

## 1. O que é e para que serve

O **Dataverse MCP Server** é um servidor MCP (Model Context Protocol) que permite que agentes de IA (GitHub Copilot, Claude, etc.) interajam diretamente com o Microsoft Dataverse em tempo real.

### O que ele faz
- **Lê** o schema do Dataverse (tabelas, colunas, relationships, option sets, solutions, security roles, teams, business units)
- **Cria/modifica** tabelas, colunas, relationships, option sets — tudo dentro de solutions gerenciadas
- **Gera código** (WebAPI calls, cURL, JavaScript, React components para PowerPages)
- **Exporta documentação** (JSON schema, diagrama ERD Mermaid)
- **Gerencia segurança** (security roles, privileges, teams, business units)

### Como funciona
```
VS Code (GitHub Copilot) → MCP Protocol → dataverse-mcp (Node.js local) → Dataverse Web API → D365 CE
```

O servidor roda localmente como um processo Node.js. Ele se autentica no Dataverse via OAuth2 Client Credentials (App Registration) e expõe 40+ ferramentas via protocolo MCP para o GitHub Copilot usar.

### Por que é útil para FTD
- Fecha gaps de investigação automaticamente (entidades ftd_*, relationships, security model)
- Gera código PowerPages para o Simulador Comercial
- Documenta o schema visualmente com Mermaid ERD
- Permite que os agentes BMAD (Tiago Dev, Wilson Architect, etc.) acessem dados reais

---

## 2. Pré-requisitos

### Na máquina de desenvolvimento
| Item | Versão mínima | Como verificar | Como instalar |
|------|--------------|----------------|---------------|
| **Node.js** | 18+ | `node --version` | https://nodejs.org/ ou `winget install OpenJS.NodeJS.LTS` |
| **npm** | 9+ | `npm --version` | Vem com Node.js |
| **Git** | Qualquer | `git --version` | `winget install Git.Git` |
| **VS Code** | 1.96+ | Menu Help > About | https://code.visualstudio.com/ |
| **GitHub Copilot** | Extensão ativa | Extensions panel | Marketplace do VS Code |

### Acessos necessários (pedir ao time FTD)
| Acesso | Quem tem | Para que |
|--------|----------|---------|
| **Azure AD — App Registration** | Azure AD Admin / Global Admin | Criar o app registration |
| **Power Platform Admin Center** | Power Platform Admin | Criar Application User |
| **Dataverse URL** | Qualquer dev FTD | É público: `https://[org].crm.dynamics.com` |

### Informações que você vai coletar
Anote esses 4 valores — serão usados na configuração:

| Valor | Onde encontrar | Exemplo |
|-------|---------------|---------|
| **DATAVERSE_URL** | Power Platform Admin Center → Environments → Environment URL | `https://ftd-dev.crm.dynamics.com` |
| **DATAVERSE_TENANT_ID** | Azure Portal → Azure AD → Overview → Tenant ID | `12345678-abcd-efgh-ijkl-123456789012` |
| **DATAVERSE_CLIENT_ID** | Azure Portal → App Registration → Overview → Application (client) ID | `abcdefgh-1234-5678-abcd-123456789012` |
| **DATAVERSE_CLIENT_SECRET** | Azure Portal → App Registration → Certificates & secrets → valor do secret | `xYz~AbCdEfGhIjKlMnOpQrStUvWxYz123456` |

---

## 3. FASE 1: Azure App Registration

### Passo 1.1 — Acessar Azure Portal
1. Abrir https://portal.azure.com/
2. Fazer login com conta que tenha permissão de **Application Administrator** ou **Global Administrator**

### Passo 1.2 — Criar App Registration
1. No menu lateral, buscar **"Azure Active Directory"** (ou **"Microsoft Entra ID"**)
2. Clicar em **"App registrations"** no menu lateral
3. Clicar em **"+ New registration"**
4. Preencher:
   - **Name**: `FTD Dataverse MCP Server`
   - **Supported account types**: `Accounts in this organizational directory only` (single tenant)
   - **Redirect URI**: Deixar em branco (não é necessário)
5. Clicar em **"Register"**

### Passo 1.3 — Anotar IDs
Na página do app registration recém-criado:
- Copiar **Application (client) ID** → este é o `DATAVERSE_CLIENT_ID`
- Copiar **Directory (tenant) ID** → este é o `DATAVERSE_TENANT_ID`

### Passo 1.4 — Criar Client Secret
1. No menu lateral do app registration, clicar em **"Certificates & secrets"**
2. Clicar em **"+ New client secret"**
3. Preencher:
   - **Description**: `MCP Server - Dev`
   - **Expires**: `24 months` (ou conforme política da FTD)
4. Clicar em **"Add"**
5. **IMEDIATAMENTE** copiar o **Value** do secret (coluna "Value", NÃO "Secret ID")
   - ⚠️ **Este valor só é exibido uma vez!** Se perder, precisa criar outro secret
   - Este é o `DATAVERSE_CLIENT_SECRET`

### Passo 1.5 — Configurar API Permissions (opcional mas recomendado)
1. No menu lateral, clicar em **"API permissions"**
2. Clicar em **"+ Add a permission"**
3. Selecionar **"Dynamics CRM"**
4. Selecionar **"Application permissions"**
5. Marcar **"user_impersonation"**
6. Clicar em **"Add permissions"**
7. Clicar em **"Grant admin consent for [tenant]"** (requer Global Admin)

> **Nota**: A permissão principal é controlada pelo Security Role no Dataverse (próxima fase), não pelas API permissions do Azure AD. Mas adicionar `user_impersonation` garante que o app pode chamar a Web API.

---

## 4. FASE 2: Application User no Dataverse

### Passo 2.1 — Acessar Power Platform Admin Center
1. Abrir https://admin.powerplatform.microsoft.com/
2. Fazer login com conta de **Power Platform Administrator**

### Passo 2.2 — Navegar até Application Users
1. No menu lateral, clicar em **"Environments"**
2. Selecionar o ambiente de **desenvolvimento** da FTD (⚠️ NUNCA produção para testes iniciais)
3. Clicar em **"Settings"**
4. Em **"Users + permissions"**, clicar em **"Application users"**

### Passo 2.3 — Criar Application User
1. Clicar em **"+ New app user"**
2. Clicar em **"+ Add an app"**
3. Buscar pelo **Client ID** (Application ID) que você anotou no Passo 1.3
4. Selecionar o app **"FTD Dataverse MCP Server"** que aparece
5. Em **"Business unit"**: selecionar a **Business Unit raiz** (geralmente o nome da organização)
6. Clicar em **"Create"**

### Passo 2.4 — Atribuir Security Role
1. Na lista de Application users, localizar o user recém-criado
2. Clicar nele para abrir
3. Clicar em **"Manage roles"** (ou "Edit security roles")
4. Selecionar o role adequado:

| Role | Quando usar | Nível de acesso |
|------|------------|-----------------|
| **System Customizer** | ✅ **Recomendado para dev/discovery** — lê schema, cria customizações | Leitura total de metadata + escrita de customizações |
| **System Administrator** | Testes iniciais — acesso total | Tudo (USE COM CUIDADO) |
| **Custom Role** | Produção — criar role específico | Só o necessário (least privilege) |

5. Clicar em **"Save"**

### Passo 2.5 — Verificar
- O Application User deve aparecer com:
  - **Application ID** = seu Client ID
  - **Status** = Enabled
  - **User type** = Application
  - **Security Role** = o que você atribuiu

### Passo 2.6 — Obter Dataverse URL
1. Ainda no Power Platform Admin Center → Environments → selecionar o ambiente
2. A **Environment URL** aparece no painel de detalhes
3. Formato: `https://[org-name].crm.dynamics.com`
4. Este é o `DATAVERSE_URL`

> **Dica alternativa**: Acessar make.powerapps.com → ícone de engrenagem → Session details → Instance url

---

## 5. FASE 3: Instalação Local

### Passo 3.1 — Escolher diretório de instalação
Escolha um diretório **fora do workspace FTD** para o MCP server. Sugestão:

```powershell
# Criar diretório para ferramentas MCP
mkdir C:\tools\mcp-servers
cd C:\tools\mcp-servers
```

### Passo 3.2 — Clonar o repositório
```powershell
git clone https://github.com/mwhesse/dataverse-mcp.git
cd dataverse-mcp
```

### Passo 3.3 — Instalar dependências
```powershell
npm install
```

Saída esperada: `added XXX packages in XXs` (sem erros)

### Passo 3.4 — Buildar o servidor
```powershell
npm run build
```

Saída esperada: compilação TypeScript sem erros, gera pasta `build/`

### Passo 3.5 — Verificar o build
```powershell
# Verificar que o arquivo principal existe
Test-Path .\build\index.js
# Deve retornar: True
```

### Passo 3.6 — Anotar o caminho completo
```powershell
# Copiar o caminho completo do index.js
(Resolve-Path .\build\index.js).Path
```

Anote este caminho (ex: `C:\tools\mcp-servers\dataverse-mcp\build\index.js`). Será usado na configuração do VS Code.

### Passo 3.7 — (Opcional) Testar com .env
```powershell
# Copiar template de .env
Copy-Item .env.example .env
```

Editar o arquivo `.env` com os 4 valores que você coletou:
```env
DATAVERSE_URL=https://ftd-dev.crm.dynamics.com
DATAVERSE_TENANT_ID=12345678-abcd-efgh-ijkl-123456789012
DATAVERSE_CLIENT_ID=abcdefgh-1234-5678-abcd-123456789012
DATAVERSE_CLIENT_SECRET=xYz~AbCdEfGhIjKlMnOpQrStUvWxYz123456
```

Testar manualmente:
```powershell
node .\build\index.js
```

Se aparecer erro de "stdin" ou "MCP transport", está correto — o servidor espera ser chamado via protocolo MCP, não diretamente. O importante é **não ter erro de autenticação**.

---

## 6. FASE 4: Configuração no VS Code

### Passo 4.1 — Abrir o workspace FTD
```powershell
code "C:\cd\Avanade IA\FTD"
```

### Passo 4.2 — Criar/editar .vscode/mcp.json
No workspace FTD, criar o arquivo `.vscode/mcp.json`:

```json
{
  "mcpServers": {
    "dataverse": {
      "command": "cmd",
      "args": [
        "/c",
        "node",
        "C:\\tools\\mcp-servers\\dataverse-mcp\\build\\index.js"
      ],
      "env": {
        "DATAVERSE_URL": "https://ftd-dev.crm.dynamics.com",
        "DATAVERSE_CLIENT_ID": "SEU-CLIENT-ID-AQUI",
        "DATAVERSE_CLIENT_SECRET": "SEU-CLIENT-SECRET-AQUI",
        "DATAVERSE_TENANT_ID": "SEU-TENANT-ID-AQUI"
      },
      "alwaysAllow": [
        "get_dataverse_table",
        "list_dataverse_tables",
        "get_dataverse_column",
        "list_dataverse_columns",
        "get_dataverse_relationship",
        "list_dataverse_relationships",
        "get_dataverse_optionset",
        "list_dataverse_optionsets",
        "get_dataverse_optionset_options",
        "get_dataverse_solution",
        "get_dataverse_publisher",
        "list_dataverse_solutions",
        "list_dataverse_publishers",
        "get_solution_context",
        "get_dataverse_role",
        "list_dataverse_roles",
        "get_role_privileges",
        "get_dataverse_team",
        "list_dataverse_teams",
        "get_team_members",
        "get_dataverse_businessunit",
        "list_dataverse_businessunits",
        "get_businessunit_hierarchy",
        "get_businessunit_users",
        "get_businessunit_teams",
        "export_solution_schema",
        "generate_mermaid_diagram",
        "generate_webapi_call",
        "generate_powerpages_webapi_call"
      ],
      "timeout": 900
    }
  }
}
```

#### ⚠️ NOTAS IMPORTANTES sobre a configuração:

1. **Caminho do index.js**: Usar barras duplas `\\` no Windows
2. **`cmd /c node`**: No Windows, DEVE usar `cmd` com `/c` para executar Node.js corretamente
3. **`timeout: 900`**: 15 minutos — necessário para operações pesadas como export de schema
4. **`alwaysAllow`**: Lista APENAS ferramentas **read-only**. Ferramentas de escrita (create, update, delete) vão pedir confirmação antes de executar

### Passo 4.3 — Adicionar ao .gitignore
O arquivo `mcp.json` contém credenciais sensíveis. **NUNCA** deve ir para o repositório:

Verificar/adicionar no arquivo `.gitignore` do workspace:
```gitignore
# MCP Server credentials
.vscode/mcp.json
```

### Passo 4.4 — Reiniciar VS Code
Fechar e reabrir o VS Code completamente para que ele detecte o novo MCP server.

### Passo 4.5 — Verificar MCP Server ativo
1. Abrir o chat do Copilot (`Ctrl+Alt+I`)
2. Na parte inferior do chat, deve aparecer um ícone de ferramentas/MCP
3. Clicar nele — o servidor **"dataverse"** deve estar listado com status **verde/ativo**
4. Expandir para ver as 40+ ferramentas disponíveis

Se o servidor aparecer com status **vermelho/erro**, vá para a seção [Troubleshooting](#11-troubleshooting).

---

## 7. FASE 5: Validação da Instalação

Execute esses testes em ordem no chat do GitHub Copilot (Agent Mode):

### Teste 1 — Listar tabelas
```
Liste todas as tabelas custom do Dataverse
```
**Esperado**: O agente chama `list_dataverse_tables` com `customOnly: true` e retorna uma lista de tabelas com prefixo (ex: `ftd_proposal`, `ftd_customer`, etc.)

### Teste 2 — Descrever uma tabela
```
Quais colunas tem a tabela [nome de uma tabela do Teste 1]?
```
**Esperado**: O agente chama `list_dataverse_columns` e retorna colunas com tipo, required level, etc.

### Teste 3 — Listar solutions
```
Quais solutions unmanaged existem no ambiente?
```
**Esperado**: `list_dataverse_solutions` retorna solutions com publisher info

### Teste 4 — Listar security roles
```
Quais security roles custom existem?
```
**Esperado**: `list_dataverse_roles` retorna roles customizados

### Teste 5 — Exportar schema
```
Exporte o schema completo das tabelas custom para JSON
```
**Esperado**: `export_solution_schema` gera um arquivo JSON com todas as tabelas, colunas e option sets

### Teste 6 — Gerar diagrama ERD
```
Gere um diagrama Mermaid ERD a partir do schema exportado
```
**Esperado**: `generate_mermaid_diagram` gera um arquivo `.mmd` com o diagrama visual

Se todos os 6 testes passarem, a instalação está completa e funcional. ✅

---

## 8. Catálogo Completo de Ferramentas

### 8.1 Table Operations (5 tools)

| Ferramenta | Tipo | O que faz |
|---|---|---|
| `create_dataverse_table` | ✏️ Escrita | Cria tabela com config completa (ownership, activities, notes, audit, AutoNumber no primary name) |
| `get_dataverse_table` | 👁️ Leitura | Retorna metadata detalhada de uma tabela |
| `update_dataverse_table` | ✏️ Escrita | Atualiza propriedades (display name, description, features) |
| `delete_dataverse_table` | 🗑️ Exclusão | Remove tabela permanentemente (⚠️ irreversível) |
| `list_dataverse_tables` | 👁️ Leitura | Lista tabelas com filtros (custom/system, managed/unmanaged) |

### 8.2 Column Operations (5 tools)

| Ferramenta | Tipo | O que faz |
|---|---|---|
| `create_dataverse_column` | ✏️ Escrita | Cria coluna em 11 tipos (String, Integer, Decimal, Money, Boolean, DateTime, Picklist, Lookup, Memo, Double, BigInt) |
| `get_dataverse_column` | 👁️ Leitura | Retorna metadata de coluna com tipo e configuração |
| `update_dataverse_column` | ✏️ Escrita | Atualiza display name, description, required level |
| `delete_dataverse_column` | 🗑️ Exclusão | Remove coluna (⚠️ perde dados da coluna) |
| `list_dataverse_columns` | 👁️ Leitura | Lista colunas de uma tabela com filtros |

### 8.3 AutoNumber Column Operations (6 tools)

| Ferramenta | Tipo | O que faz |
|---|---|---|
| `create_autonumber_column` | ✏️ Escrita | Cria coluna AutoNumber (ex: `PRJ-{SEQNUM:5}`) |
| `update_autonumber_format` | ✏️ Escrita | Altera formato do AutoNumber |
| `set_autonumber_seed` | ✏️ Escrita | Define valor inicial da sequência |
| `get_autonumber_column` | 👁️ Leitura | Retorna info do AutoNumber |
| `list_autonumber_columns` | 👁️ Leitura | Lista AutoNumbers de uma tabela ou todas |
| `convert_to_autonumber` | ✏️ Escrita | Converte coluna texto existente em AutoNumber |

**Placeholders de formato**:
- `{SEQNUM:n}` → número sequencial com n dígitos (ex: 00001)
- `{RANDSTRING:n}` → string aleatória com n chars (1-6)
- `{DATETIMEUTC:format}` → data/hora UTC (ex: yyyyMMdd)

### 8.4 Relationship Operations (4 tools)

| Ferramenta | Tipo | O que faz |
|---|---|---|
| `create_dataverse_relationship` | ✏️ Escrita | Cria 1:N (com lookup) ou N:N (com junction table), com cascade behaviors |
| `get_dataverse_relationship` | 👁️ Leitura | Retorna detalhes do relationship |
| `delete_dataverse_relationship` | 🗑️ Exclusão | Remove relationship (⚠️ remove lookup se 1:N) |
| `list_dataverse_relationships` | 👁️ Leitura | Lista relationships com filtros |

### 8.5 Option Set Operations (6 tools)

| Ferramenta | Tipo | O que faz |
|---|---|---|
| `create_dataverse_optionset` | ✏️ Escrita | Cria option set global com opções, cores, valores |
| `get_dataverse_optionset` | 👁️ Leitura | Retorna metadata do option set |
| `update_dataverse_optionset` | ✏️ Escrita | Adiciona/remove/atualiza opções |
| `delete_dataverse_optionset` | 🗑️ Exclusão | Remove option set (falha se em uso) |
| `list_dataverse_optionsets` | 👁️ Leitura | Lista option sets com filtros |
| `get_dataverse_optionset_options` | 👁️ Leitura | Retorna valores, labels, cores de cada opção |

### 8.6 Solution & Publisher Operations (8 tools)

| Ferramenta | Tipo | O que faz |
|---|---|---|
| `create_dataverse_publisher` | ✏️ Escrita | Cria publisher com prefix de customização (ex: "ftd") |
| `get_dataverse_publisher` | 👁️ Leitura | Retorna detalhes do publisher |
| `list_dataverse_publishers` | 👁️ Leitura | Lista publishers |
| `create_dataverse_solution` | ✏️ Escrita | Cria solution vinculada a um publisher |
| `get_dataverse_solution` | 👁️ Leitura | Retorna detalhes da solution |
| `list_dataverse_solutions` | 👁️ Leitura | Lista solutions |
| `set_solution_context` | ✏️ Escrita | Define solution ativa (tudo criado depois vai para ela) |
| `get_solution_context` | 👁️ Leitura | Retorna solution ativa atual |
| `clear_solution_context` | ✏️ Escrita | Limpa contexto de solution |

**IMPORTANTE**: O contexto de solution é **persistido** em arquivo `.dataverse-mcp` no diretório do projeto. Sobrevive a reinícios do servidor.

### 8.7 Security Role Operations (13 tools)

| Ferramenta | Tipo | O que faz |
|---|---|---|
| `create_dataverse_role` | ✏️ Escrita | Cria security role |
| `get_dataverse_role` | 👁️ Leitura | Retorna detalhes do role |
| `update_dataverse_role` | ✏️ Escrita | Atualiza nome, descrição, auto-assignment |
| `delete_dataverse_role` | 🗑️ Exclusão | Remove role (falha se atribuído) |
| `list_dataverse_roles` | 👁️ Leitura | Lista roles com filtros |
| `add_privileges_to_role` | ✏️ Escrita | Adiciona privileges com access level (Basic/Local/Deep/Global) |
| `remove_privilege_from_role` | ✏️ Escrita | Remove privilege específico |
| `replace_role_privileges` | ✏️ Escrita | Substitui TODOS os privileges (⚠️ destrutivo) |
| `get_role_privileges` | 👁️ Leitura | Lista todos os privileges de um role |
| `assign_role_to_user` | ✏️ Escrita | Atribui role a um usuário |
| `remove_role_from_user` | ✏️ Escrita | Remove role de um usuário |
| `assign_role_to_team` | ✏️ Escrita | Atribui role a um team |
| `remove_role_from_team` | ✏️ Escrita | Remove role de um team |

### 8.8 Team Operations (9 tools)

| Ferramenta | Tipo | O que faz |
|---|---|---|
| `create_dataverse_team` | ✏️ Escrita | Cria team (Owner ou Access) |
| `get_dataverse_team` | 👁️ Leitura | Retorna detalhes |
| `update_dataverse_team` | ✏️ Escrita | Atualiza propriedades |
| `delete_dataverse_team` | 🗑️ Exclusão | Remove team |
| `list_dataverse_teams` | 👁️ Leitura | Lista teams com filtros |
| `add_members_to_team` | ✏️ Escrita | Adiciona membros |
| `remove_members_from_team` | ✏️ Escrita | Remove membros |
| `get_team_members` | 👁️ Leitura | Lista membros do team |
| `convert_owner_team_to_access_team` | ✏️ Escrita | Converte tipo (⚠️ irreversível) |

### 8.9 Business Unit Operations (9 tools)

| Ferramenta | Tipo | O que faz |
|---|---|---|
| `create_dataverse_businessunit` | ✏️ Escrita | Cria BU com endereço, contato, hierarquia |
| `get_dataverse_businessunit` | 👁️ Leitura | Retorna detalhes |
| `update_dataverse_businessunit` | ✏️ Escrita | Atualiza propriedades |
| `delete_dataverse_businessunit` | 🗑️ Exclusão | Remove BU |
| `list_dataverse_businessunits` | 👁️ Leitura | Lista BUs |
| `get_businessunit_hierarchy` | 👁️ Leitura | Retorna árvore hierárquica |
| `set_businessunit_parent` | ✏️ Escrita | Move BU na hierarquia |
| `get_businessunit_users` | 👁️ Leitura | Lista usuários da BU |
| `get_businessunit_teams` | 👁️ Leitura | Lista teams da BU |

### 8.10 Schema Export & Visualization (2 tools)

| Ferramenta | Tipo | O que faz |
|---|---|---|
| `export_solution_schema` | 👁️ Leitura | Exporta JSON completo (tabelas, colunas, option sets) com filtros por prefix, sistema, etc. |
| `generate_mermaid_diagram` | 👁️ Leitura | Gera ERD Mermaid a partir do JSON exportado (com PK, FK, markers, relationships) |

### 8.11 WebAPI Call Generator (1 tool)

| Ferramenta | Tipo | O que faz |
|---|---|---|
| `generate_webapi_call` | 👁️ Leitura | Gera HTTP request + cURL + JavaScript para operações: retrieve, retrieveMultiple, create, update, delete, associate, disassociate, callAction, callFunction |

### 8.12 PowerPages WebAPI Generator (1 tool)

| Ferramenta | Tipo | O que faz |
|---|---|---|
| `generate_powerpages_webapi_call` | 👁️ Leitura | Gera chamadas `/_api/` para PowerPages SPAs com: schema-aware metadata, @odata.bind, React components, request verification token, auth context |

### 8.13 PowerPages Configuration Management (1 tool)

| Ferramenta | Tipo | O que faz |
|---|---|---|
| `manage_powerpages_webapi_config` | ✏️ Escrita | Gerencia: status, configure-webapi, create-table-permission, list-configurations. Automatiza YAML de `.powerpages-site/` |

---

## 9. Workflows de Uso Prático

### 9.1 Discovery: Mapear schema existente (READ-ONLY)

Conversa com o agente:
```
1. "Liste todas as tabelas custom do Dataverse"
   → list_dataverse_tables (customOnly: true)

2. "Quais colunas tem a tabela ftd_proposal?"
   → list_dataverse_columns (entityLogicalName: "ftd_proposal")

3. "Quais relationships existem para ftd_proposal?"
   → list_dataverse_relationships (filtering by entity)

4. "Quais option sets custom existem?"
   → list_dataverse_optionsets (customOnly: true)

5. "Exporte o schema completo para JSON e gere um ERD Mermaid"
   → export_solution_schema + generate_mermaid_diagram
```

### 9.2 Criar schema novo em solution

Conversa com o agente:
```
1. "Crie um publisher 'ftd' com prefix 'ftd' e option value prefix 10000"
   → create_dataverse_publisher

2. "Crie uma solution 'FTD Simulador v2' vinculada ao publisher ftd"
   → create_dataverse_solution

3. "Defina essa solution como contexto ativo"
   → set_solution_context

4. "Crie a tabela 'Proposta Comercial' com AutoNumber no primary name formato PROP-{SEQNUM:5}"
   → create_dataverse_table

5. "Adicione as colunas: Cliente (Lookup para account), Valor Total (Money), Status (Picklist: Rascunho/Submetido/Aprovado/Recusado), Data Submissão (DateTime)"
   → create_dataverse_column (múltiplas chamadas)

6. "Crie um relationship 1:N de account para ftd_propostacomercial"
   → create_dataverse_relationship

7. "Exporte o schema e gere ERD"
   → export_solution_schema + generate_mermaid_diagram
```

### 9.3 Auditar modelo de segurança

```
1. "Liste todos os security roles custom"
   → list_dataverse_roles (customOnly: true)

2. "Quais privileges tem o role 'FTD Vendedor'?"
   → get_role_privileges

3. "Quais teams existem e quem são os membros?"
   → list_dataverse_teams + get_team_members

4. "Qual a hierarquia de business units?"
   → list_dataverse_businessunits + get_businessunit_hierarchy
```

### 9.4 Gerar código para PowerPages (Simulador)

```
1. "Gere o código React para listar propostas comerciais no PowerPages"
   → generate_powerpages_webapi_call (operation: retrieveMultiple)

2. "Gere o código para criar uma nova proposta com lookup para account"
   → generate_powerpages_webapi_call (operation: create, com @odata.bind)

3. "Verifique se a tabela ftd_proposal está habilitada para WebAPI no PowerPages"
   → manage_powerpages_webapi_config (operation: status)

4. "Habilite WebAPI para ftd_proposal com campos nome, valor, status"
   → manage_powerpages_webapi_config (operation: configure-webapi)

5. "Crie table permission para Authenticated Users com leitura e criação"
   → manage_powerpages_webapi_config (operation: create-table-permission)
```

### 9.5 Gerar WebAPI calls para Azure Functions

```
1. "Gere um HTTP request para buscar propostas com status 'Submetido' ordenadas por data"
   → generate_webapi_call (operation: retrieveMultiple, filter, orderby)

2. "Gere o cURL para criar um novo registro de proposta"
   → generate_webapi_call (operation: create, com data)

3. "Gere o JavaScript fetch para chamar a action WinOpportunity"
   → generate_webapi_call (operation: callAction)
```

---

## 10. Configuração de Segurança (alwaysAllow)

### Conceito
O `alwaysAllow` define quais ferramentas o agente pode executar **sem pedir confirmação**. Ferramentas fora dessa lista exibem um popup pedindo aprovação.

### Recomendação: Só read-only no alwaysAllow
Todas as ferramentas que começam com `get_`, `list_`, `export_`, `generate_` são **somente leitura** — seguras para auto-aprovar.

Ferramentas que começam com `create_`, `update_`, `delete_`, `set_`, `add_`, `remove_`, `replace_`, `convert_`, `clear_`, `assign_`, `manage_` **modificam dados** — devem pedir confirmação.

### Configuração mínima segura (somente leitura)
A lista no Passo 4.2 já está configurada com apenas as ferramentas read-only.

### Configuração agressiva (dev playground)
Se estiver em ambiente de **desenvolvimento** descartável e quiser velocidade máxima:
```json
"alwaysAllow": ["*"]
```
⚠️ **NUNCA** use `"*"` em ambientes com dados reais ou em produção.

---

## 11. Troubleshooting

### Erro: "Authentication Failed"
**Causa**: Client ID, Secret ou Tenant ID incorretos
**Solução**:
1. Verificar os 4 valores no `.vscode/mcp.json`
2. Confirmar que o Client Secret não expirou
3. Testar autenticação manualmente:
```powershell
$body = @{
    grant_type    = "client_credentials"
    client_id     = "SEU-CLIENT-ID"
    client_secret = "SEU-SECRET"
    resource      = "https://ftd-dev.crm.dynamics.com/"
}
Invoke-RestMethod -Uri "https://login.microsoftonline.com/SEU-TENANT-ID/oauth2/token" -Method Post -Body $body
```
Se retornar um JSON com `access_token`, a autenticação funciona.

### Erro: "Permission Denied" / "403 Forbidden"
**Causa**: Application User sem Security Role adequado
**Solução**:
1. Ir ao Power Platform Admin Center → Application users
2. Verificar que o user está **Enabled**
3. Verificar que tem **System Customizer** (ou superior) atribuído
4. Verificar que o **Application ID** no Dataverse bate com o Client ID do Azure

### Erro: "Entity Not Found"
**Causa**: Nome lógico da tabela incorreto
**Solução**: Usar `list_dataverse_tables` primeiro para obter os nomes corretos

### MCP Server não aparece no VS Code
**Causas possíveis**:
1. Caminho do `index.js` incorreto no `mcp.json` → verificar com `Test-Path`
2. Node.js não está no PATH → verificar com `node --version`
3. VS Code não recarregou → fechar e reabrir completamente
4. Extensão GitHub Copilot desatualizada → verificar updates

### Timeout em operações longas
**Causa**: Schema export em ambientes grandes pode demorar
**Solução**: O `timeout: 900` (15 min) já está configurado. Se ainda não for suficiente, aumentar para `1800` (30 min).

### Debug Mode
Para logs verbosos, adicionar ao env no `mcp.json`:
```json
"env": {
    "DEBUG": "true",
    ...outros vars...
}
```

---

## 12. Boas Práticas

### Ambiente
- ✅ **SEMPRE** usar ambiente de **desenvolvimento** para testes iniciais
- ✅ Criar Application User separado para cada ambiente (DEV, QA, PROD)
- ❌ **NUNCA** apontar para produção com `alwaysAllow: ["*"]`

### Credenciais
- ✅ Client Secret no `mcp.json` que está no `.gitignore`
- ✅ Rotacionar secrets a cada 6-12 meses
- ❌ **NUNCA** commitar credenciais no repositório
- ❌ **NUNCA** compartilhar credenciais por chat/email

### Solution Context
- ✅ **SEMPRE** setar solution context antes de criar schema
- ✅ Usar publisher com prefix consistente (ex: "ftd")
- ✅ O contexto persiste em `.dataverse-mcp` — adicionar ao `.gitignore`
- ❌ **NUNCA** criar tabelas/colunas sem solution context (vão parar na Default Solution)

### Naming Conventions
- ✅ displayName em português: "Proposta Comercial"
- ✅ logicalName auto-gerado: `ftd_propostacomercial` (prefix + lowercase, sem espaços)
- ✅ Usar option value prefix consistente por publisher (ex: 10000-19999 para ftd)

### Workflow de ALM
```
1. DEV: Criar/modificar schema via MCP (solution context ativo)
2. DEV: Exportar schema → JSON + Mermaid ERD para documentação
3. DEV: Testar com dados de teste
4. EXPORT: Via Power Platform Admin Center ou pac CLI, exportar solution como managed
5. QA: Importar solution managed no ambiente de QA
6. PROD: Após aprovação, importar em produção
```

---

## 13. Integração com Avanade Method

### Como os agentes BMAD se beneficiam

| Agente | Uso do MCP |
|--------|-----------|
| **Maria Analyst** | Discovery: `list_dataverse_tables`, `list_dataverse_columns` → entender schema existente para análise de requisitos |
| **João PM** | PRDs: `describe_table` → referenciar campos reais nos requisitos |
| **Wilson Architect** | ADRs: `export_solution_schema` + `generate_mermaid_diagram` → documentar arquitetura baseada em dados reais |
| **Paula PO** | Stories: `list_dataverse_columns` → escrever ACs com nomes de campos corretos |
| **Tiago Dev** | Implementação: `create_dataverse_table/column` + `generate_webapi_call` + `generate_powerpages_webapi_call` → criar schema e gerar código |
| **Carla QA** | Code Review: `list_dataverse_columns` → validar que código usa nomes corretos |
| **Sofia UX** | Wireframes: `list_dataverse_columns` → mapear campos de formulário com dados reais |
| **Roberto SM** | Sprint Planning: `list_dataverse_tables` → estimar complexidade baseada em schema real |

### Contexto automático
Uma vez configurado, o MCP está disponível para TODOS os agentes via chatmodes. Nenhuma configuração adicional por agente é necessária. O GitHub Copilot detecta as ferramentas MCP automaticamente e as usa quando o prompt do usuário exige acesso a dados do Dataverse.

---

## Referências

- **Repositório**: https://github.com/mwhesse/dataverse-mcp
- **Releases**: https://github.com/mwhesse/dataverse-mcp/releases
- **Changelog**: https://github.com/mwhesse/dataverse-mcp/blob/main/CHANGELOG.md
- **Documentação Dataverse Web API**: https://learn.microsoft.com/en-us/power-apps/developer/data-platform/webapi/overview
- **MCP Protocol**: https://modelcontextprotocol.io/introduction
- **Azure App Registration**: https://learn.microsoft.com/en-us/entra/identity-platform/quickstart-register-app
