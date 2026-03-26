# Análise de Ferramentas e Riscos — mwhesse/dataverse-mcp

**Versão**: 1.0 | **Data**: 2026-03-25
**Projeto**: FTD Educação — Avanade Method
**Repositório**: https://github.com/mwhesse/dataverse-mcp (v0.2.8, MIT License)
**Objetivo**: Inventário completo de ferramentas e análise de riscos para o cliente

---

## 1. Resumo Executivo

O **mwhesse/dataverse-mcp** é um servidor MCP (Model Context Protocol) comunitário que expõe **70+ ferramentas** para interação com o Microsoft Dataverse via GitHub Copilot/VS Code. Permite leitura de schema, criação/modificação de entidades, gestão de segurança e geração de código.

| Métrica | Valor |
|---|---|
| Total de ferramentas | **70+** |
| Ferramentas de leitura (seguras) | **29** |
| Ferramentas de escrita (risco) | **32** |
| Ferramentas de exclusão (alto risco) | **9** |
| Stars no GitHub | 15 |
| Linguagem | TypeScript / Node.js |
| Licença | MIT |

---

## 2. Catálogo Completo de Ferramentas

### 2.1 Table Operations (5 tools)

| # | Ferramenta | Tipo | Descrição | Nível de Risco |
|---|---|---|---|---|
| 1 | `list_dataverse_tables` | 👁️ Leitura | Lista tabelas com filtros (custom/system, managed/unmanaged) | Nenhum |
| 2 | `get_dataverse_table` | 👁️ Leitura | Retorna metadata detalhada de uma tabela | Nenhum |
| 3 | `create_dataverse_table` | ✏️ Escrita | Cria tabela com config completa (ownership, activities, notes, audit, AutoNumber) | **Médio** |
| 4 | `update_dataverse_table` | ✏️ Escrita | Atualiza propriedades (display name, description, features) | **Médio** |
| 5 | `delete_dataverse_table` | 🗑️ Exclusão | Remove tabela permanentemente | **CRÍTICO** |

### 2.2 Column Operations (5 tools)

| # | Ferramenta | Tipo | Descrição | Nível de Risco |
|---|---|---|---|---|
| 6 | `list_dataverse_columns` | 👁️ Leitura | Lista colunas de uma tabela com filtros | Nenhum |
| 7 | `get_dataverse_column` | 👁️ Leitura | Retorna metadata de coluna com tipo e configuração | Nenhum |
| 8 | `create_dataverse_column` | ✏️ Escrita | Cria coluna (11 tipos: String, Integer, Decimal, Money, Boolean, DateTime, Picklist, Lookup, Memo, Double, BigInt) | **Médio** |
| 9 | `update_dataverse_column` | ✏️ Escrita | Atualiza display name, description, required level | **Médio** |
| 10 | `delete_dataverse_column` | 🗑️ Exclusão | Remove coluna — perda de dados da coluna | **ALTO** |

### 2.3 AutoNumber Column Operations (6 tools)

| # | Ferramenta | Tipo | Descrição | Nível de Risco |
|---|---|---|---|---|
| 11 | `get_autonumber_column` | 👁️ Leitura | Retorna info do AutoNumber | Nenhum |
| 12 | `list_autonumber_columns` | 👁️ Leitura | Lista AutoNumbers de uma tabela ou todas | Nenhum |
| 13 | `create_autonumber_column` | ✏️ Escrita | Cria coluna AutoNumber (ex: `PRJ-{SEQNUM:5}`) | **Médio** |
| 14 | `update_autonumber_format` | ✏️ Escrita | Altera formato do AutoNumber | **Médio** |
| 15 | `set_autonumber_seed` | ✏️ Escrita | Define valor inicial da sequência — pode causar duplicatas ou gaps | **Médio** |
| 16 | `convert_to_autonumber` | ✏️ Escrita | Converte coluna texto existente em AutoNumber — irreversível | **ALTO** |

### 2.4 Relationship Operations (4 tools)

| # | Ferramenta | Tipo | Descrição | Nível de Risco |
|---|---|---|---|---|
| 17 | `list_dataverse_relationships` | 👁️ Leitura | Lista relationships com filtros | Nenhum |
| 18 | `get_dataverse_relationship` | 👁️ Leitura | Retorna detalhes do relationship | Nenhum |
| 19 | `create_dataverse_relationship` | ✏️ Escrita | Cria 1:N (com lookup) ou N:N (com junction table), com cascade behaviors | **Médio** |
| 20 | `delete_dataverse_relationship` | 🗑️ Exclusão | Remove relationship — remove lookup associado se 1:N | **ALTO** |

### 2.5 Option Set Operations (6 tools)

| # | Ferramenta | Tipo | Descrição | Nível de Risco |
|---|---|---|---|---|
| 21 | `list_dataverse_optionsets` | 👁️ Leitura | Lista option sets com filtros | Nenhum |
| 22 | `get_dataverse_optionset` | 👁️ Leitura | Retorna metadata do option set | Nenhum |
| 23 | `get_dataverse_optionset_options` | 👁️ Leitura | Retorna valores, labels, cores de cada opção | Nenhum |
| 24 | `create_dataverse_optionset` | ✏️ Escrita | Cria option set global com opções, cores, valores | **Baixo** |
| 25 | `update_dataverse_optionset` | ✏️ Escrita | Adiciona/remove/atualiza opções — pode afetar registros existentes | **Médio** |
| 26 | `delete_dataverse_optionset` | 🗑️ Exclusão | Remove option set (falha se em uso) | **Médio** |

### 2.6 Solution & Publisher Operations (9 tools)

| # | Ferramenta | Tipo | Descrição | Nível de Risco |
|---|---|---|---|---|
| 27 | `list_dataverse_solutions` | 👁️ Leitura | Lista solutions | Nenhum |
| 28 | `get_dataverse_solution` | 👁️ Leitura | Retorna detalhes da solution | Nenhum |
| 29 | `list_dataverse_publishers` | 👁️ Leitura | Lista publishers | Nenhum |
| 30 | `get_dataverse_publisher` | 👁️ Leitura | Retorna detalhes do publisher | Nenhum |
| 31 | `get_solution_context` | 👁️ Leitura | Retorna solution ativa atual | Nenhum |
| 32 | `create_dataverse_publisher` | ✏️ Escrita | Cria publisher com prefix de customização | **Baixo** |
| 33 | `create_dataverse_solution` | ✏️ Escrita | Cria solution vinculada a um publisher | **Baixo** |
| 34 | `set_solution_context` | ✏️ Escrita | Define solution ativa (tudo criado depois vai para ela) | **Baixo** |
| 35 | `clear_solution_context` | ✏️ Escrita | Limpa contexto de solution | **Baixo** |

### 2.7 Security Role Operations (13 tools)

| # | Ferramenta | Tipo | Descrição | Nível de Risco |
|---|---|---|---|---|
| 36 | `list_dataverse_roles` | 👁️ Leitura | Lista roles com filtros | Nenhum |
| 37 | `get_dataverse_role` | 👁️ Leitura | Retorna detalhes do role | Nenhum |
| 38 | `get_role_privileges` | 👁️ Leitura | Lista todos os privileges de um role | Nenhum |
| 39 | `create_dataverse_role` | ✏️ Escrita | Cria security role | **Médio** |
| 40 | `update_dataverse_role` | ✏️ Escrita | Atualiza nome, descrição, auto-assignment | **Médio** |
| 41 | `delete_dataverse_role` | 🗑️ Exclusão | Remove role (falha se atribuído) | **ALTO** |
| 42 | `add_privileges_to_role` | ✏️ Escrita | Adiciona privileges com access level (Basic/Local/Deep/Global) | **ALTO** |
| 43 | `remove_privilege_from_role` | ✏️ Escrita | Remove privilege específico — pode bloquear usuários | **ALTO** |
| 44 | `replace_role_privileges` | ✏️ Escrita | Substitui TODOS os privileges de uma vez | **CRÍTICO** |
| 45 | `assign_role_to_user` | ✏️ Escrita | Atribui role a um usuário — altera permissões de acesso | **ALTO** |
| 46 | `remove_role_from_user` | ✏️ Escrita | Remove role de um usuário — pode bloquear acesso | **ALTO** |
| 47 | `assign_role_to_team` | ✏️ Escrita | Atribui role a um team inteiro | **ALTO** |
| 48 | `remove_role_from_team` | ✏️ Escrita | Remove role de um team — pode bloquear toda a equipe | **ALTO** |

### 2.8 Team Operations (9 tools)

| # | Ferramenta | Tipo | Descrição | Nível de Risco |
|---|---|---|---|---|
| 49 | `list_dataverse_teams` | 👁️ Leitura | Lista teams com filtros | Nenhum |
| 50 | `get_dataverse_team` | 👁️ Leitura | Retorna detalhes do team | Nenhum |
| 51 | `get_team_members` | 👁️ Leitura | Lista membros do team | Nenhum |
| 52 | `create_dataverse_team` | ✏️ Escrita | Cria team (Owner ou Access) | **Médio** |
| 53 | `update_dataverse_team` | ✏️ Escrita | Atualiza propriedades | **Médio** |
| 54 | `delete_dataverse_team` | 🗑️ Exclusão | Remove team | **ALTO** |
| 55 | `add_members_to_team` | ✏️ Escrita | Adiciona membros ao team | **Médio** |
| 56 | `remove_members_from_team` | ✏️ Escrita | Remove membros do team | **Médio** |
| 57 | `convert_owner_team_to_access_team` | ✏️ Escrita | Converte tipo de team — operação irreversível | **CRÍTICO** |

### 2.9 Business Unit Operations (9 tools)

| # | Ferramenta | Tipo | Descrição | Nível de Risco |
|---|---|---|---|---|
| 58 | `list_dataverse_businessunits` | 👁️ Leitura | Lista Business Units | Nenhum |
| 59 | `get_dataverse_businessunit` | 👁️ Leitura | Retorna detalhes da BU | Nenhum |
| 60 | `get_businessunit_hierarchy` | 👁️ Leitura | Retorna árvore hierárquica | Nenhum |
| 61 | `get_businessunit_users` | 👁️ Leitura | Lista usuários da BU | Nenhum |
| 62 | `get_businessunit_teams` | 👁️ Leitura | Lista teams da BU | Nenhum |
| 63 | `create_dataverse_businessunit` | ✏️ Escrita | Cria BU com endereço, contato, hierarquia | **Médio** |
| 64 | `update_dataverse_businessunit` | ✏️ Escrita | Atualiza propriedades | **Médio** |
| 65 | `delete_dataverse_businessunit` | 🗑️ Exclusão | Remove Business Unit | **ALTO** |
| 66 | `set_businessunit_parent` | ✏️ Escrita | Move BU na hierarquia — afeta herança de segurança | **ALTO** |

### 2.10 Schema Export & Visualização (2 tools)

| # | Ferramenta | Tipo | Descrição | Nível de Risco |
|---|---|---|---|---|
| 67 | `export_solution_schema` | 👁️ Leitura | Exporta JSON completo (tabelas, colunas, option sets) | Nenhum |
| 68 | `generate_mermaid_diagram` | 👁️ Leitura | Gera ERD Mermaid a partir do JSON exportado | Nenhum |

### 2.11 WebAPI Call Generator (1 tool)

| # | Ferramenta | Tipo | Descrição | Nível de Risco |
|---|---|---|---|---|
| 69 | `generate_webapi_call` | 👁️ Leitura | Gera HTTP request + cURL + JavaScript para operações CRUD, actions, functions | Nenhum |

### 2.12 PowerPages WebAPI Generator (1 tool)

| # | Ferramenta | Tipo | Descrição | Nível de Risco |
|---|---|---|---|---|
| 70 | `generate_powerpages_webapi_call` | 👁️ Leitura | Gera chamadas `/_api/` para PowerPages SPAs com React components | Nenhum |

### 2.13 PowerPages Configuration Management (1 tool)

| # | Ferramenta | Tipo | Descrição | Nível de Risco |
|---|---|---|---|---|
| 71 | `manage_powerpages_webapi_config` | ✏️ Escrita | Gerencia: status, configure-webapi, create-table-permission, list-configurations | **ALTO** |

---

## 3. Distribuição de Risco

```
┌──────────────────────────────────────────────────────────────┐
│                  DISTRIBUIÇÃO POR TIPO                        │
├──────────────────────────────────────────────────────────────┤
│  👁️ Leitura (29 tools)     ████████████████████░░░░░░  41%  │
│  ✏️ Escrita (32 tools)     ██████████████████████░░░░  45%  │
│  🗑️ Exclusão (9 tools)     ██████░░░░░░░░░░░░░░░░░░░  13%  │
└──────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────┐
│                DISTRIBUIÇÃO POR NÍVEL DE RISCO                │
├──────────────────────────────────────────────────────────────┤
│  ✅ Nenhum (29 tools)      ████████████████████░░░░░░  41%  │
│  🟢 Baixo (6 tools)        ████░░░░░░░░░░░░░░░░░░░░░   8%  │
│  🟡 Médio (19 tools)       █████████████░░░░░░░░░░░░░  27%  │
│  🟠 Alto (14 tools)        ██████████░░░░░░░░░░░░░░░░  20%  │
│  🔴 Crítico (3 tools)      ██░░░░░░░░░░░░░░░░░░░░░░░░   4%  │
└──────────────────────────────────────────────────────────────┘
```

---

## 4. Análise de Riscos para o Cliente (FTD Educação)

### 4.1 Riscos CRÍTICOS (podem causar danos irreversíveis)

| # | Risco | Ferramentas Envolvidas | Impacto | Probabilidade |
|---|---|---|---|---|
| R1 | **Exclusão acidental de tabelas/colunas** | `delete_dataverse_table`, `delete_dataverse_column` | Perda permanente de dados e customizações do CRM | Média (se `alwaysAllow` mal configurado) |
| R2 | **Escalação de privilégios** | `replace_role_privileges`, `add_privileges_to_role`, `assign_role_to_user` | Usuários com acesso indevido a dados sensíveis (contratos, valores, clientes) | Baixa (requer ação intencional) |
| R3 | **Conversão irreversível de teams** | `convert_owner_team_to_access_team`, `convert_to_autonumber` | Sem rollback possível — necessita recriação manual | Baixa |
| R4 | **Exposição de credenciais** | Arquivo `.vscode/mcp.json` com Client Secret | Se commitado no Git, compromete o ambiente Dataverse inteiro | Alta (erro humano comum) |

### 4.2 Riscos ALTOS

| # | Risco | Detalhe | Impacto |
|---|---|---|---|
| R5 | **Alteração de schema em produção** | Se o MCP estiver apontando para PROD em vez de DEV, cria/altera tabelas em produção | Interrupção de operações dos consultores e vendedores FTD |
| R6 | **Bloqueio de usuários** | `remove_role_from_user/team` pode impedir acesso de consultores/vendedores ao CRM | Paralisação das equipes comerciais |
| R7 | **Reestruturação de Business Units** | `set_businessunit_parent` altera a hierarquia organizacional, afetando herança de segurança | Mudança de visibilidade de dados entre áreas |
| R8 | **Exposição de dados via PowerPages** | `manage_powerpages_webapi_config` pode abrir tabelas sensíveis para usuários externos do portal (Simulador) | Vazamento de dados de clientes/propostas |

### 4.3 Riscos da FERRAMENTA em si (open-source)

| # | Risco | Detalhe | Severidade |
|---|---|---|---|
| R9 | **Projeto comunitário com baixa adoção** | 15 stars no GitHub — sem SLA de suporte, sem garantia de manutenção futura | Média |
| R10 | **Supply chain risk (npm)** | Dependências npm podem conter pacotes maliciosos ou vulneráveis | Média |
| R11 | **Credenciais em texto plano** | Client Secret armazenado em texto plano no `.vscode/mcp.json` local | Alta |
| R12 | **Sem audit trail nativo** | Ações executadas pelo MCP não geram logs próprios — auditoria depende dos logs do Dataverse | Média |
| R13 | **Sem versionamento de alterações** | Alterações feitas via MCP no schema não passam pelo pipeline de ALM (Guard build → Deploy) | Alta |

---

## 5. Matriz de Risco (Impacto × Probabilidade)

```
      IMPACTO →    Baixo      Médio        Alto        Crítico
PROBABILIDADE ↓
    Alta          │          │  R11       │  R13       │  R4
    Média         │          │  R9, R10   │  R5, R12   │  R1
    Baixa         │          │            │  R6, R7, R8│  R2, R3
```

---

## 6. Ferramentas Seguras para `alwaysAllow` (Recomendação)

As seguintes ferramentas são **somente leitura** e podem ser auto-aprovadas sem risco:

```json
"alwaysAllow": [
  "get_dataverse_table",
  "list_dataverse_tables",
  "get_dataverse_column",
  "list_dataverse_columns",
  "get_autonumber_column",
  "list_autonumber_columns",
  "get_dataverse_relationship",
  "list_dataverse_relationships",
  "get_dataverse_optionset",
  "list_dataverse_optionsets",
  "get_dataverse_optionset_options",
  "get_dataverse_solution",
  "list_dataverse_solutions",
  "get_dataverse_publisher",
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
]
```

> **NUNCA** adicionar ao `alwaysAllow`: `create_*`, `update_*`, `delete_*`, `set_*`, `add_*`, `remove_*`, `replace_*`, `convert_*`, `clear_*`, `assign_*`, `manage_*`

---

## 7. Mitigações Recomendadas

### 7.1 Controles Obrigatórios

| # | Mitigação | Risco(s) tratado(s) | Prioridade |
|---|---|---|---|
| M1 | **`.vscode/mcp.json` no `.gitignore`** — verificar ANTES de qualquer commit | R4 | **Imediata** |
| M2 | **Usar APENAS em ambiente DEV** — nunca apontar para PROD ou OAT | R1, R5, R6, R7, R8 | **Imediata** |
| M3 | **`alwaysAllow` somente leitura** — ferramentas de escrita pedem confirmação | R1, R2, R6 | **Imediata** |
| M4 | **Security Role mínimo**: usar **System Customizer** (nunca System Administrator) | R2, R5 | **Imediata** |

### 7.2 Controles Recomendados

| # | Mitigação | Risco(s) tratado(s) | Prioridade |
|---|---|---|---|
| M5 | **Criar Custom Security Role** para produção com least privilege | R2 | Alta |
| M6 | **Rotacionar Client Secret** a cada 6-12 meses | R4, R11 | Alta |
| M7 | **`npm audit`** periódico nas dependências do MCP | R10 | Média |
| M8 | **Revisar logs do Dataverse** (Audit) para ações feitas via MCP | R12 | Média |
| M9 | **Criar Application User separado** por ambiente (DEV, QA, PROD) | R5 | Alta |
| M10 | **Considerar migração para MCP oficial Microsoft** (`microsoft/Dataverse-MCP`) no médio prazo | R9 | Média |

### 7.3 Processo de ALM (Application Lifecycle Management)

```
1. DEV: Criar/modificar schema via MCP (com solution context ativo)
2. DEV: Exportar schema → JSON + Mermaid ERD para documentação
3. DEV: Testar com dados de teste
4. EXPORT: Via Power Platform Admin Center ou PAC CLI, exportar solution como managed
5. QA: Importar solution managed no ambiente de QA
6. PROD: Após aprovação (GMUD), importar em produção via pipeline
```

> O MCP **NÃO deve ser usado** para deploy direto em PROD. O fluxo de ALM (pipeline YAML) deve ser respeitado conforme padrão BCA Avanade.

---

## 8. Comparação: mwhesse vs. MCP Oficial Microsoft

| Critério | mwhesse/dataverse-mcp | microsoft/Dataverse-MCP |
|---|---|---|
| **Autor** | Comunidade (1 dev) | Microsoft (Daniel Laskewitz) |
| **Stars** | 15 | 48 |
| **Suporte** | Sem SLA | Oficial Microsoft |
| **Tools** | 70+ (mais completo) | Menor escopo |
| **PowerPages** | Sim (geração de código React) | Não |
| **Security roles/teams/BU** | Sim (13+ tools) | Limitado |
| **Mermaid ERD** | Sim | Não |
| **Risco supply chain** | Médio (npm, comunidade) | Baixo (Microsoft) |
| **Recomendação** | DEV/Discovery imediato | Longo prazo / governança |

---

## 9. Recomendação por Cenário de Uso

| Cenário | Seguro? | Recomendação |
|---|---|---|
| **Discovery / leitura de schema** (list, get, export) | ✅ Seguro | Usar livremente em DEV |
| **Gerar código** (WebAPI, PowerPages React) | ✅ Seguro | Output é código — não executa nada no Dataverse |
| **Gerar diagrama Mermaid ERD** | ✅ Seguro | Excelente para documentação da arquitetura |
| **Criar schema em DEV** (create table/column) | ⚠️ Cuidado | Revisar cada confirmação antes de aprovar |
| **Alterar option sets existentes** | ⚠️ Cuidado | Pode afetar registros existentes |
| **Alterar security roles** | ❌ Alto risco | Preferir fazer manualmente no Admin Center |
| **Gerenciar teams e business units** | ❌ Alto risco | Alterações afetam hierarquia de segurança |
| **Usar em PRODUÇÃO** | ❌ Proibido | Nunca — usar pipelines de ALM |
| **`alwaysAllow: ["*"]`** | ❌ Proibido | Nunca em ambientes com dados reais |

---

## 10. Conclusão

O **mwhesse/dataverse-mcp** é a ferramenta mais completa disponível para integração Dataverse + IA (VS Code/Copilot). Para o projeto FTD Educação, ele oferece valor significativo em:

- **Discovery**: Mapeamento automático do schema existente (entidades `ftd_*`, relationships, security model)
- **Documentação**: Geração de ERD Mermaid e exportação de schema JSON
- **Simulador Comercial**: Geração de código PowerPages WebAPI + React components
- **Produtividade**: Eliminação de consultas manuais à Web API

**Pré-condição para uso**: Os 4 controles obrigatórios (M1-M4) devem ser implementados **antes** de disponibilizar a ferramenta para qualquer membro do time.

**Alternativa de longo prazo**: Monitorar o `microsoft/Dataverse-MCP` (oficial) para migração quando atingir paridade de funcionalidades.

---

*Documento gerado em 2026-03-25 — Projeto FTD Educação — Avanade Method*
