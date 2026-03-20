---

## 📋 O que é este Template?

Template especializado para **PRD de customizações Dynamics 365 CE**. Estende o PRD padrão com seções específicas para Dataverse model, plugins, Power Platform e integração Azure.

**Quando usar**: Toda vez que uma customização D365 CE for planejada — funcionalidades que envolvem mudanças em entidades, plugins, flows, PCF controls ou integrações.

---

## 📝 D365 CE PRD Template

```yaml
# ============================================================
# PRODUCT REQUIREMENTS DOCUMENT - DYNAMICS 365 CE
# ============================================================
# Version: 1.0 (Avanade Method v4.29.0 - D365 Context)
# Template Owner: João PM
# Stack: Dynamics 365 CE + Power Platform + Azure

metadata:
  document_id: "PRD-D365-YYYY-MM-###"
  title: "[Nome da Customização/Feature]"
  version: "1.0"
  status: "Draft | In Review | Approved | Archived"
  created_date: "YYYY-MM-DD"
  d365_module: "[Sales | Service | Marketing | Field Service | Cross-module]"
  solution_name: "[Nome da solution D365 onde será empacotado]"

# ============================================================
# 1. EXECUTIVE SUMMARY (padrão PRD + contexto D365)
# ============================================================
executive_summary:
  problem_statement: |
    [Qual problema CRM/processo de negócio estamos resolvendo?]
  proposed_solution: |
    [Como vamos resolver usando D365 CE + Power Platform?]
  expected_impact: |
    [Benefícios para usuários CRM e processos de negócio]
  d365_approach:
    configuration_percentage: "[%] do escopo é configuração OOB"
    low_code_percentage: "[%] do escopo é Power Automate / Canvas Apps"
    pro_code_percentage: "[%] do escopo é Plugins / PCF / Azure Functions"

# ============================================================
# 2. D365 SCOPE - DATAVERSE MODEL
# ============================================================
dataverse_model:
  new_tables:
    - logical_name: "[prefixo]_[nome]"
      display_name: "[Nome para Exibição]"
      description: "[Propósito da tabela]"
      ownership: "User/Team | Organization"
      primary_column: "[Nome da coluna principal]"
      key_columns:
        - name: "[coluna]"
          type: "[Lookup | Text | Number | DateTime | Choice | etc]"
          required: true/false
          description: "[Para quê]"

  modified_tables:
    - logical_name: "[tabela existente - ex: account, contact, opportunity]"
      changes:
        new_columns:
          - name: "[prefixo]_[coluna]"
            type: "[tipo]"
            description: "[propósito]"
        modified_columns:
          - name: "[coluna existente]"
            change: "[O que muda]"
        new_relationships:
          - type: "[1:N | N:1 | N:N]"
            related_table: "[tabela relacionada]"
            description: "[natureza da relação]"

  views:
    - table: "[tabela]"
      name: "[Nome da View]"
      type: "System | Personal"
      columns: ["col1", "col2", "col3"]
      filters: "[Descrição dos filtros]"
      sort: "[Ordenação]"

  forms:
    - table: "[tabela]"
      name: "[Nome do Form]"
      type: "Main | Quick Create | Quick View"
      layout:
        header: ["campo1", "campo2"]
        tabs:
          - name: "[Nome da Tab]"
            sections:
              - name: "[Seção]"
                columns: ["campo1", "campo2"]
          - name: "[Outra Tab]"
            sections:
              - name: "[Seção com sub-grid]"
                subgrid: "[tabela relacionada]"
      business_rules:
        - name: "[Nome da BR]"
          condition: "[Se campo X = valor]"
          action: "[Então mostrar/ocultar/setar campo Y]"

# ============================================================
# 3. D365 SCOPE - BUSINESS LOGIC
# ============================================================
business_logic:
  plugins:
    - name: "[Namespace.Entity.Message]"
      entity: "[logical_name da tabela]"
      message: "[Create | Update | Delete | Associate | Custom]"
      stage: "[Pre-Validation(10) | Pre-Operation(20) | Post-Operation(40)]"
      execution: "[Synchronous | Asynchronous]"
      filtering_attributes: ["atributo1", "atributo2"]
      description: |
        [O que este plugin faz - regra de negócio]
      acceptance_criteria:
        - "[Critério 1 - Given/When/Then]"
        - "[Critério 2]"

  custom_apis:
    - name: "[Nome da Custom API]"
      description: "[O que faz]"
      request_parameters:
        - name: "[param]"
          type: "[String | Entity | EntityReference | etc]"
      response_properties:
        - name: "[prop]"
          type: "[tipo]"
      use_case: "[Quando/por que chamar esta API]"

  business_rules:
    - table: "[tabela]"
      name: "[Nome]"
      scope: "Entity | All Forms | Specific Form"
      condition: "[Se...]"
      action: "[Então...]"

  calculated_fields:
    - table: "[tabela]"
      field: "[campo]"
      formula: "[Fórmula de cálculo]"

  rollup_fields:
    - table: "[tabela]"
      field: "[campo]"
      related_table: "[tabela filha]"
      aggregation: "[SUM | COUNT | AVG | MIN | MAX]"
      filter: "[Critério de filtro]"

# ============================================================
# 4. D365 SCOPE - AUTOMAÇÃO (Power Automate)
# ============================================================
automation:
  cloud_flows:
    - name: "FTD - [Módulo] - [Ação] - [Trigger]"
      trigger:
        type: "[Dataverse | Schedule | HTTP | Manual]"
        details: "[Quando row criada/atualizada em tabela X]"
      actions:
        - "[Passo 1: O que faz]"
        - "[Passo 2: O que faz]"
      error_handling: "[Estratégia de erro]"
      concurrency: "[Parallel | Sequential]"
      acceptance_criteria:
        - "[Critério 1]"

  approval_flows:
    - name: "[Nome do fluxo de aprovação]"
      trigger: "[Quando disparar]"
      approvers: "[Quem aprova - role/field]"
      levels: "[Número de níveis de aprovação]"
      timeout: "[Tempo limite]"
      escalation: "[Para quem escalar]"

# ============================================================
# 5. D365 SCOPE - UI COMPONENTS
# ============================================================
ui_components:
  pcf_controls:
    - name: "FTD[NomeControl]Control"
      description: "[O que faz / onde é usado]"
      bound_field: "[Campo/tabela onde será usado]"
      technology: "[React | Virtual DOM | etc]"
      inputs: ["[param1]", "[param2]"]
      outputs: ["[output1]"]
      form_factor: "[Web | Tablet | Phone]"

  web_resources:
    - name: "ftd_/scripts/[módulo]/[arquivo].js"
      type: "[JavaScript | HTML | CSS | Image]"
      purpose: "[O que faz]"
      events: ["OnLoad", "OnChange(campo)", "OnSave"]

  ribbon_buttons:
    - name: "[Nome do botão]"
      location: "[Command Bar | Sub-grid | etc]"
      visibility_rule: "[Quando mostrar]"
      action: "[O que faz ao clicar]"

  canvas_apps:
    - name: "[Nome do Canvas App]"
      embedded_in: "[Form D365 | Standalone | Teams]"
      data_source: "[Dataverse | SharePoint | Custom API]"
      screens:
        - name: "[Tela 1]"
          purpose: "[O que faz]"

  power_pages:
    - name: "[Nome do Portal/Page]"
      audience: "[Clientes | Parceiros | Funcionários]"
      features:
        - "[Funcionalidade 1]"
      authentication: "[Azure AD B2C | Local | etc]"

# ============================================================
# 6. D365 SCOPE - INTEGRAÇÃO
# ============================================================
integration:
  azure_functions:
    - name: "FTD.Functions.[Domínio].[Função]"
      trigger: "[HTTP | ServiceBus | Timer | EventGrid]"
      purpose: "[O que faz]"
      dataverse_connection: "[Managed Identity | App Registration]"
      external_systems: ["[Sistema 1]", "[Sistema 2]"]

  service_bus:
    - entity: "[Queue | Topic]"
      name: "[Nome]"
      publisher: "[Quem envia - Plugin / Function / Flow]"
      subscriber: "[Quem recebe - Function / External]"
      message_schema: "[Formato da mensagem]"

  webhooks:
    - entity: "[Tabela Dataverse]"
      message: "[Create | Update | Delete]"
      endpoint: "[URL do Azure Function]"
      authentication: "[Header | QueryString | HttpHeader]"

  data_sync:
    - source: "[Sistema de origem]"
      target: "[Sistema de destino]"
      frequency: "[Real-time | Batch (schedule)]"
      mechanism: "[Azure Function | Power Automate | Data Integration]"
      conflict_resolution: "[Last write wins | Source wins | Manual]"

# ============================================================
# 7. SECURITY MODEL
# ============================================================
security:
  security_roles:
    - name: "[Nome do Security Role]"
      based_on: "[Role base - ex: Salesperson]"
      permissions:
        - table: "[tabela]"
          create: "[User | BU | Parent-Child | Org | None]"
          read: "[User | BU | Parent-Child | Org | None]"
          write: "[User | BU | Parent-Child | Org | None]"
          delete: "[User | BU | Parent-Child | Org | None]"
          append: "[User | BU | Parent-Child | Org | None]"
          append_to: "[User | BU | Parent-Child | Org | None]"

  field_security_profiles:
    - name: "[Nome do Profile]"
      fields:
        - table: "[tabela]"
          field: "[campo]"
          read: true/false
          update: true/false
          create: true/false

  business_units:
    - name: "[Nome do BU]"
      purpose: "[Segregação de dados para...]"

# ============================================================
# 8. ALM & DEPLOYMENT
# ============================================================
alm:
  solutions:
    - name: "[Nome da Solution]"
      publisher: "[Publisher]"
      type: "[Core | Module | Integration]"
      components:
        - "[Tipo: tables, plugins, flows, etc]"
      dependencies:
        - "[Solution da qual depende]"

  environments:
    dev: "[URL do environment de dev]"
    test: "[URL do environment de test/QA]"
    uat: "[URL do environment de UAT]"
    prod: "[URL do environment de produção]"

  deployment_plan:
    - step: 1
      action: "[Export unmanaged de Dev]"
    - step: 2
      action: "[Build managed via pipeline]"
    - step: 3
      action: "[Import managed em Test]"
    - step: 4
      action: "[Validação QA]"
    - step: 5
      action: "[Import managed em UAT]"
    - step: 6
      action: "[Validação PO/Business]"
    - step: 7
      action: "[Import managed em Prod]"

# ============================================================
# 9. DATA MIGRATION (se aplicável)
# ============================================================
data_migration:
  scope:
    - source: "[Sistema de origem]"
      tables: ["[tabela1]", "[tabela2]"]
      estimated_records: "[Volume estimado]"
  mapping:
    - source_field: "[Campo origem]"
      target_table: "[Tabela Dataverse]"
      target_field: "[Campo destino]"
      transformation: "[Regra de transformação se houver]"
  strategy: "[Big bang | Phased | Parallel run]"
  cutover_plan: "[Plano de corte]"
```

---

## 📖 Guia de Preenchimento D365

### Decisão: Configuration vs Low-code vs Pro-code

| Necessidade | Abordagem | Ferramenta |
|-------------|-----------|------------|
| Novo campo/tabela | Configuration | Dataverse Designer |
| Validação simples no form | Configuration | Business Rules |
| Valor calculado | Configuration | Calculated/Rollup Field |
| Lógica server-side complexa | Pro-code | Plugin C# |
| Operação reutilizável | Pro-code | Custom API |
| Notificação automática | Low-code | Power Automate |
| Aprovação multi-nível | Low-code | Power Automate Approvals |
| UI customizada complexa | Pro-code | PCF Control |
| Script de form simples | Pro-code | JavaScript Web Resource |
| Portal externo | Low-code + Pro-code | Power Pages |
| Integração com API externa | Pro-code | Azure Function |
| Processamento assíncrono pesado | Pro-code | Azure Function + Service Bus |
| App mobile customizado | Low-code | Canvas App |
| Dashboard complexo | Configuration | System Dashboard |

### Story Points Reference D365

| Points | Tipo de Trabalho | Exemplo |
|--------|-----------------|---------|
| 1 | Config simples | Adicionar campo em form existente |
| 2 | Config moderada | Criar view com filtros, business rule simples |
| 3 | Low-code | Power Automate flow com 3-5 steps |
| 5 | Pro-code simples | Plugin de validação Pre-Operation |
| 8 | Pro-code moderado | PCF Control novo, Azure Function com integração |
| 13 | Pro-code complexo | Plugin complexo com lógica multi-entidade + testes |
| 21+ | Dividir! | Integração completa com sistema externo |

---
