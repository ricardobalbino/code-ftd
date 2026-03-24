# Pesquisa: MCPs para Dynamics 365 / Power Platform / Dataverse

**Data**: 2026-03-14
**Contexto**: Pesquisa de MCPs disponíveis para contextualização de agentes IA no projeto FTD Educação

---

## 📌 RESUMO EXECUTIVO

Existem **3 camadas** de MCP disponíveis para o ecossistema D365/Power Platform:
1. **Microsoft Oficial** - Dataverse Remote MCP Server (lançado no Build)
2. **Copilot Studio** - Suporte nativo a MCP servers (remote, Streamable HTTP)
3. **Comunidade** - 5+ servidores MCP com 40-51 tools cada

**IMPORTANTE**: O comando `pac mcp` **NÃO existe** na documentação oficial do PAC CLI (verificado na referência completa de comandos). Foi um equívoco da análise anterior.

---

## 1. MICROSOFT OFICIAL: Dataverse Remote MCP Server

**Repositório**: https://github.com/microsoft/Dataverse-MCP
**Status**: Lançado no Microsoft Build | 48 stars | MIT License
**Autor**: Daniel Laskewitz (Microsoft)

### O que é
- MCP Server **remoto** oficial da Microsoft para Dataverse
- Funciona como proxy local que se conecta ao Dataverse remoto
- Tem labs prontos para Claude Desktop e VS Code

### Labs Disponíveis
1. **Lab 01**: Setup do ambiente Dataverse + instalação do MCP Local Proxy
2. **Lab 02**: Usar Dataverse MCP com Claude Desktop
3. **Lab 03**: Usar Dataverse MCP com VS Code + GitHub Copilot

### User Guide
- https://go.microsoft.com/fwlink/?linkid=2320176

### Relevância FTD: ⭐⭐⭐⭐⭐
- Oficial Microsoft = suporte garantido
- Integração direta com VS Code = funciona com nosso BMAD
- Pode fechar vários gaps de investigação (entidades, plugins, security roles)

---

## 2. COPILOT STUDIO: Suporte Nativo a MCP

**Docs**: https://learn.microsoft.com/en-us/microsoft-copilot-studio/agent-extend-action-mcp

### O que faz
- Copilot Studio suporta MCP servers via **Streamable HTTP** transport
- SSE transport **descontinuado** (agosto 2025)
- Generative Orchestration precisa estar habilitado
- Suporta **Tools** e **Resources** do MCP

### Formas de Conectar
1. **MCP Onboarding Wizard** (recomendado): Direto no Copilot Studio
2. **Custom Connector via Power Apps**: Para cenários avançados com OpenAPI schema

### Autenticação Suportada
- None (sem auth)
- API Key (header ou query)
- OAuth 2.0 (Dynamic Discovery, Dynamic, Manual)

### Relevância FTD: ⭐⭐⭐
- Útil se FTD quiser criar agentes Copilot Studio que acessam dados externos via MCP
- Não é o foco principal (nosso foco é VSCode + BMAD)

---

## 3. AZURE AI FOUNDRY: Integrar MCP Tools com Azure AI Agents

**Training**: https://learn.microsoft.com/en-us/training/modules/connect-agent-to-mcp-tools/

### O que faz
- Module de treinamento para integrar MCP Tools com Azure AI Agents
- Explica como wrappear MCP tools como funções assíncronas
- Registrar tools em Azure AI agents via Foundry SDK

### Relevância FTD: ⭐⭐
- Aplicável se FTD quiser construir agentes Azure AI que acessam Dataverse
- Futuro, não imediato

---

## 4. COMUNIDADE: mwhesse/dataverse-mcp ⭐ RECOMENDADO

**Repositório**: https://github.com/mwhesse/dataverse-mcp
**Stars**: 15 | **Versão**: v0.2.8 | **Linguagem**: TypeScript
**Última atualização**: Agosto 2025

### 🚀 ESTE É O MAIS COMPLETO - 40+ TOOLS

#### Table Operations
- `create_dataverse_table` ✅
- `get_dataverse_table` ✅
- `update_dataverse_table` ✅
- `delete_dataverse_table` ✅
- `list_dataverse_tables` ✅

#### Column Operations
- `create_dataverse_column` (11 tipos: String, Integer, Decimal, Money, Boolean, DateTime, Picklist, Lookup, Memo, Double, BigInt)
- `get_dataverse_column`
- `update_dataverse_column`
- `delete_dataverse_column`
- `list_dataverse_columns`

#### AutoNumber Column Operations
- `create_autonumber_column`
- `update_autonumber_format`
- `set_autonumber_seed`
- `get_autonumber_column`
- `list_autonumber_columns`
- `convert_to_autonumber`

#### Relationship Operations
- `create_dataverse_relationship` (One-to-Many, Many-to-Many)
- `get_dataverse_relationship`
- `delete_dataverse_relationship`
- `list_dataverse_relationships`

#### Option Set Operations
- `create_dataverse_optionset`
- `get_dataverse_optionset`
- `update_dataverse_optionset`
- `delete_dataverse_optionset`
- `list_dataverse_optionsets`
- `get_dataverse_optionset_options`

#### Solution & Publisher Operations
- `create_dataverse_publisher`/`get`/`list`
- `create_dataverse_solution`/`get`/`list`
- `set_solution_context`/`get`/`clear`

#### Security Role Operations (13 ferramentas!)
- `create_dataverse_role`/`get`/`update`/`delete`/`list`
- `add_privileges_to_role`/`remove`/`replace`/`get`
- `assign_role_to_user`/`remove`
- `assign_role_to_team`/`remove`

#### Team Operations
- `create_dataverse_team`/`get`/`update`/`delete`/`list`
- `add_members_to_team`/`remove`/`get_team_members`
- `convert_owner_team_to_access_team`

#### Business Unit Operations
- `create_dataverse_businessunit`/`get`/`update`/`delete`/`list`
- `get_businessunit_hierarchy`/`set_parent`
- `get_businessunit_users`/`teams`

#### Schema Export & Visualization
- `export_solution_schema` → JSON completo
- `generate_mermaid_diagram` → ERD profissional

#### WebAPI Call Generator
- `generate_webapi_call` → HTTP requests, cURL, JavaScript para qualquer operação CRUD

#### PowerPages WebAPI Generator 🤩
- `generate_powerpages_webapi_call` → Chamadas /_api/ para PowerPages SPAs
- Schema-aware com @odata.bind
- Gera React components prontos

#### PowerPages Configuration Management
- `manage_powerpages_webapi_config` → Table permissions, WebAPI site settings

### Requisitos
- Azure App Registration com Client Credentials
- Application User no Dataverse com security roles
- Node.js para build

### Config VS Code (Windows)
```json
{
  "mcpServers": {
    "dataverse": {
      "command": "cmd",
      "args": ["/c", "node", "C:\\path\\to\\dataverse-mcp\\build\\index.js"],
      "env": {
        "DATAVERSE_URL": "https://yourorg.crm.dynamics.com",
        "DATAVERSE_CLIENT_ID": "your-client-id",
        "DATAVERSE_CLIENT_SECRET": "your-client-secret",
        "DATAVERSE_TENANT_ID": "your-tenant-id"
      },
      "alwaysAllow": [
        "get_dataverse_table", "list_dataverse_tables",
        "get_dataverse_column", "list_dataverse_columns",
        "get_dataverse_relationship", "list_dataverse_relationships",
        "get_dataverse_optionset", "list_dataverse_optionsets",
        "get_solution_context", "list_dataverse_solutions",
        "export_solution_schema", "generate_mermaid_diagram",
        "generate_webapi_call", "generate_powerpages_webapi_call"
      ],
      "timeout": 900
    }
  }
}
```

### Relevância FTD: ⭐⭐⭐⭐⭐ (MÁXIMA)
- **Power Pages (Simulador Comercial)**: Gera WebAPI calls e React components
- **Schema Discovery**: Fecha GAPs de investigação (entidades ftd_*, relationships)
- **Security Roles**: Mapear modelo de segurança atual
- **Solutions**: Listar/analisar solutions existentes
- **Mermaid ERD**: Documentação visual automática do schema
- **Funciona com VS Code**: Integra direto no BMAD

---

## 5. OUTROS MCPs COMUNIDADE

### rajyraman/mcp-dataverse
- **Repo**: https://github.com/rajyraman/mcp-dataverse
- **Stars**: 10 | **C#** | Abril 2025
- **O que faz**: Query Dataverse usando **SQL** (TDS endpoint)
- **Relevância FTD**: ⭐⭐⭐ — Consultas SQL são úteis para análise de dados

### Cliveo/Power-Platform-MCP
- **Repo**: https://github.com/Cliveo/Power-Platform-MCP
- **Stars**: 8 | **.NET/C#**
- **O que faz**: Integração completa com Power Platform services via GitHub Copilot
- **Relevância FTD**: ⭐⭐⭐ — Alternativa .NET se preferirmos C#

### codeurali/mcp-dataverse
- **Repo**: https://github.com/codeurali/mcp-dataverse
- **Stars**: 2 | **JavaScript** | Atualizado recentemente
- **O que faz**: 51 tools para Dataverse Web API (query, CRUD, metadata, search, files, audit)
- **Relevância FTD**: ⭐⭐⭐ — Mais tools que o mwhesse, mas menos maduro

### bonanip512/DataverseMCPServer
- **Repo**: https://github.com/bonanip512/DataverseMCPServer
- **Stars**: 7 | **TypeScript**
- **O que faz**: Node.js MCP + HTTP endpoint para Dataverse
- **Relevância FTD**: ⭐⭐

---

## 6. PAC CLI — Situação Real

**NOTA IMPORTANTE**: Após verificar a documentação oficial completa do PAC CLI (https://learn.microsoft.com/en-us/power-platform/developer/cli/reference/), os command groups disponíveis são:

`admin`, `application`, `auth`, `canvas`, `catalog`, `code`, `connection`, `connector`, `copilot`, `data`, `env`, `help`, `managed-identity`, `model`, `modelbuilder`, `package`, `pages`, `pcf`, `pipeline`, `plugin`, `power-fx`, `solution`, `telemetry`, `test`, `tool`

**`pac mcp` NÃO aparece na lista de comandos.** A informação anterior sobre `pac mcp` era uma previsão/expectativa, não uma realidade atual. O PAC CLI pode ser usado como ferramenta complementar:

- `pac solution list` — listar solutions
- `pac data export/import` — dados
- `pac pages download/upload` — Power Pages
- `pac plugin` — gerenciar plugins
- `pac model` — model-driven apps (preview)
- `pac copilot` — ferramentas de copilot

---

## 📋 RECOMENDAÇÃO PARA FTD

### Prioridade 1: mwhesse/dataverse-mcp (IMEDIATO)
**Razão**: Mais completo, production-ready (100% tested), suporte a PowerPages (Simulador), schema export, Mermaid diagrams. Funciona direto no VS Code.

**Passos**:
1. Criar Azure App Registration para o MCP
2. Criar Application User no Dataverse FTD
3. Clonar repo, `npm install`, `npm run build`
4. Configurar `.vscode/mcp.json` no workspace FTD
5. Testar com `list_dataverse_tables` (customOnly: true)

### Prioridade 2: Microsoft/Dataverse-MCP (OFICIAL)
**Razão**: Oficial Microsoft, proxy remoto, labs prontos.

**Passos**: Seguir Lab 01 + Lab 03 do repo oficial

### Prioridade 3 (Futuro): Copilot Studio MCP
**Razão**: Quando FTD quiser expor MCP servers para agentes Copilot Studio.

---

## 🎯 GAPS DE INVESTIGAÇÃO QUE O MCP PODE FECHAR

Com o **mwhesse/dataverse-mcp**, podemos automaticamente:

| Gap | Tool MCP | Comando |
|-----|---------|---------|
| Lista exata de entidades custom (ftd_*) | `list_dataverse_tables` | customOnly: true |
| Colunas de cada entidade | `list_dataverse_columns` | entityLogicalName: "ftd_*" |
| Relationships entre entidades | `list_dataverse_relationships` | filtering by entity |
| Solutions existentes | `list_dataverse_solutions` | includeManaged: false |
| Security Roles | `list_dataverse_roles` | customOnly: true |
| Teams | `list_dataverse_teams` | - |
| Business Units | `list_dataverse_businessunits` | - |
| Schema visual completo | `export_solution_schema` + `generate_mermaid_diagram` | - |
| Power Pages config | `manage_powerpages_webapi_config` | operation: "status" |
| Gerar WebAPI calls | `generate_webapi_call` | Para cada operação |
| Gerar PowerPages React | `generate_powerpages_webapi_call` | Para Simulador |
