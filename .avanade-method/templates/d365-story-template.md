# ============================================================
# D365 STORY TEMPLATE - Avanade Method v4.29.0
# ============================================================
# Template Owner: Paula PO
# Extends: story-template.yaml com seções D365-specific

# USAGE: Para toda story de customização D365 CE.
# Selecione o story_type adequado e preencha as seções relevantes.

---

## 📋 Como usar este Template

Cada story D365 tem um `story_type` que determina quais seções são obrigatórias:

| Story Type | Seções Obrigatórias |
|-----------|-------------------|
| Configuration | dataverse_changes, forms_views_changes |
| Plugin | plugin_spec, unit_test_spec |
| PCF Control | pcf_spec, unit_test_spec |
| Azure Function | function_spec, unit_test_spec |
| Power Automate Flow | flow_spec |
| Integration | integration_spec, function_spec |
| Canvas App | canvas_app_spec |
| Power Pages | power_pages_spec |
| Security | security_spec |
| Data Migration | migration_spec |
| Web Resource | web_resource_spec |

---

```yaml
# ============================================================
# STORY HEADER
# ============================================================
story:
  id: "STORY-D365-###"
  epic_id: "EPIC-D365-###"
  title: "[Como <persona>, quero <ação> para <valor>]"
  story_type: "[Configuration | Plugin | PCF | AzureFunction | Flow | Integration | CanvasApp | PowerPages | Security | Migration | WebResource]"
  solution: "[Nome da solution onde será empacotado]"
  d365_module: "[Sales | Service | Marketing | Field Service | Cross]"
  priority: "[Must | Should | Could | Won't]"
  story_points: "[1 | 2 | 3 | 5 | 8 | 13]"
  sprint: "[Sprint N]"
  status: "[Draft | Ready | In Progress | Review | Done]"

# ============================================================
# ACCEPTANCE CRITERIA (obrigatório para todos os tipos)
# ============================================================
acceptance_criteria:
  - id: "AC-01"
    given: "[Contexto inicial]"
    when: "[Ação do usuário ou trigger]"
    then: "[Resultado esperado]"
  - id: "AC-02"
    given: "[...]"
    when: "[...]"
    then: "[...]"

# ============================================================
# DATAVERSE CHANGES (para Configuration, Plugin, etc.)
# ============================================================
dataverse_changes:
  new_columns:
    - table: "[logical_name]"
      column: "[prefix_name]"
      type: "[Text | Number | Lookup | Choice | DateTime | etc]"
      required: "[Required | Recommended | Optional]"
      description: "[Propósito]"
  modified_columns:
    - table: "[logical_name]"
      column: "[existing_column]"
      change: "[O que muda]"
  new_relationships:
    - type: "[1:N | N:1 | N:N]"
      primary: "[tabela principal]"
      related: "[tabela relacionada]"
      description: "[Natureza da relação]"

# ============================================================
# FORMS & VIEWS CHANGES
# ============================================================
forms_views_changes:
  forms:
    - table: "[tabela]"
      form: "[Main | Quick Create | Quick View]"
      changes:
        - "[Adicionar campo X na seção Y]"
        - "[Criar nova tab Z com sub-grid de...]"
  views:
    - table: "[tabela]"
      view: "[Nome da View]"
      type: "[System | Personal]"
      columns: ["col1", "col2"]
      filters: "[Filtros]"
  business_rules:
    - table: "[tabela]"
      name: "[Nome da BR]"
      scope: "[Entity | All Forms | Form específico]"
      logic: "IF [condição] THEN [ação]"

# ============================================================
# PLUGIN SPEC (story_type: Plugin)
# ============================================================
plugin_spec:
  class_name: "[Namespace.Entity.MessageStage]"
  entity: "[logical_name]"
  message: "[Create | Update | Delete | Associate | SetState | Custom]"
  stage: "[PreValidation(10) | PreOperation(20) | PostOperation(40)]"
  execution_mode: "[Synchronous | Asynchronous]"
  filtering_attributes: ["attr1", "attr2"]
  logic_description: |
    [Descrição detalhada da lógica de negócio implementada pelo plugin]
  pre_image: "[attributes necessários do pre-image, ou null]"
  post_image: "[attributes necessários do post-image, ou null]"
  shared_variables: "[variáveis compartilhadas entre plugins, ou null]"
  error_handling: |
    [Como erros de negócio devem ser tratados - InvalidPluginExecutionException messages]

# ============================================================
# UNIT TEST SPEC (para Plugin, PCF, Azure Function)
# ============================================================
unit_test_spec:
  framework: "[FakeXrmEasy + xUnit | Jest | xUnit + Moq]"
  test_cases:
    - name: "[Should_DoX_When_Y]"
      arrange: "[Setup do teste]"
      act: "[Ação executada]"
      assert: "[Verificação esperada]"
    - name: "[Should_ThrowError_When_InvalidInput]"
      arrange: "[Setup do cenário de erro]"
      act: "[Ação que causa erro]"
      assert: "[Exceção/erro esperado]"
  min_coverage: 80

# ============================================================
# PCF CONTROL SPEC (story_type: PCF)
# ============================================================
pcf_spec:
  control_name: "FTD[ControlName]Control"
  description: "[O que o controle faz]"
  bound_field_type: "[SingleLine.Text | Whole.None | etc]"
  technology: "TypeScript + React"
  manifest:
    property_name: "[nome_propriedade]"
    property_type: "[tipo_propriedade]"
    inputs: ["[input1]", "[input2]"]
    outputs: ["[output1]"]
  ux_requirements: |
    [Descrição visual e comportamental do controle]
  form_placement: "[Tabela.Form.Seção onde será colocado]"

# ============================================================
# AZURE FUNCTION SPEC (story_type: AzureFunction)
# ============================================================
function_spec:
  function_name: "FTD.Functions.[Domain].[Name]"
  trigger: "[HTTP | ServiceBusTrigger | TimerTrigger | EventGridTrigger]"
  runtime: ".NET 6+ (Isolated)"
  authentication: "[Managed Identity | App Registration | Function Key]"
  input_schema: |
    [JSON schema ou descrição dos parâmetros de entrada]
  output_schema: |
    [JSON schema ou descrição do retorno]
  dataverse_operations: |
    [Quais operações CRUD faz no Dataverse, se aplicável]
  external_apis: |
    [APIs externas que consome, se aplicável]
  error_handling: "[Retry policy, dead letter, logging]"

# ============================================================
# POWER AUTOMATE FLOW SPEC (story_type: Flow)
# ============================================================
flow_spec:
  flow_name: "FTD - [Module] - [Action] - [Trigger]"
  type: "[Automated | Scheduled | Instant | Approval]"
  trigger:
    type: "[Dataverse | Schedule | HTTP | Manual | When row added/modified]"
    details: "[Detalhes do trigger]"
  steps:
    - order: 1
      action: "[Nome da ação]"
      details: "[O que faz]"
    - order: 2
      action: "[Nome da ação]"
      details: "[O que faz]"
  error_handling: "Scope Try-Catch-Finally"
  concurrency: "[Degree of parallelism | Sequential]"
  run_as: "[Flow owner | Connection reference]"

# ============================================================
# INTEGRATION SPEC (story_type: Integration)
# ============================================================
integration_spec:
  pattern: "[Webhook | Service Bus | Polling | Batch]"
  source_system: "[Sistema de origem]"
  target_system: "[Sistema de destino]"
  direction: "[Inbound | Outbound | Bidirectional]"
  frequency: "[Real-time | Schedule (cron)]"
  message_format: "[JSON | XML | CSV]"
  idempotency: "[Como garantir idempotência]"
  error_recovery: "[Retry, dead-letter, alerting]"
  volume_estimate: "[registros/hora ou dia]"

# ============================================================
# CANVAS APP SPEC (story_type: CanvasApp)
# ============================================================
canvas_app_spec:
  app_name: "[Nome do Canvas App]"
  embedded_in: "[D365 Form | Standalone | Teams | SharePoint]"
  data_source: "[Dataverse | SharePoint | Custom Connector | SQL]"
  screens:
    - name: "[Screen Name]"
      purpose: "[O que faz]"
      components: ["Gallery", "Form", "Button"]
  delegation_aware: true
  offline_capable: false
  target_users: "[Role ou grupo de usuários]"

# ============================================================
# POWER PAGES SPEC (story_type: PowerPages)
# ============================================================
power_pages_spec:
  site_name: "[Nome do site]"
  audience: "[Clientes | Parceiros | Fornecedores | Funcionários]"
  authentication: "[Azure AD B2C | Local | Social]"
  pages:
    - name: "[Page Name]"
      purpose: "[O que faz]"
      entity_permissions: "[Tabela e nível de acesso]"
  web_roles: ["[Role1]", "[Role2]"]
  custom_javascript: "[Se necessário]"

# ============================================================
# SECURITY SPEC (story_type: Security)
# ============================================================
security_spec:
  security_roles:
    - name: "[Nome do Role]"
      based_on: "[Role base]"
      permissions:
        - table: "[tabela]"
          create: "[User | BU | ParentChild | Org | None]"
          read: "[User | BU | ParentChild | Org | None]"
          write: "[User | BU | ParentChild | Org | None]"
          delete: "[User | BU | ParentChild | Org | None]"
  field_security:
    - profile: "[Nome do profile]"
      fields:
        - table: "[tabela]"
          field: "[campo]"
          read: true
          update: false
  teams:
    - name: "[Nome do team]"
      type: "[Owner | Access | AAD Security Group]"
      roles: ["[role1]"]

# ============================================================
# DATA MIGRATION SPEC (story_type: Migration)
# ============================================================
migration_spec:
  source: "[Sistema/arquivo de origem]"
  target_table: "[Tabela Dataverse destino]"
  record_count: "[Volume estimado]"
  field_mapping:
    - source: "[campo_origem]"
      target: "[campo_destino]"
      transformation: "[Regra, se houver]"
  dedup_strategy: "[Como identificar duplicatas]"
  validation_rules: "[Regras de validação pós-import]"
  rollback_plan: "[Como reverter se necessário]"

# ============================================================
# WEB RESOURCE SPEC (story_type: WebResource)
# ============================================================
web_resource_spec:
  name: "ftd_/scripts/[module]/[file].js"
  type: "[JavaScript | HTML | CSS]"
  form: "[Tabela.FormName]"
  events:
    - event: "[OnLoad | OnChange | OnSave | Ribbon]"
      field: "[campo, se OnChange]"
      handler: "[functionName]"
  dependencies: ["[other web resources]"]
  api_calls: "[Dataverse Web API calls necessárias]"

# ============================================================
# DEV AGENT RECORD (preenchido pelo Tiago Dev)
# ============================================================
dev_agent_record:
  implementation_notes: ""
  files_created: []
  files_modified: []
  test_results: ""
  solution_components_added: []
  plugin_steps_registered: []
```
