---
applyTo: "**/*.cs,**/*.ts,**/*.js,**/*.yaml,**/*.yml,**/*.xml,**/*.json,**/*.ps1"
---

# Avanade BCA (BizApps Core Accelerator) — Diretrizes de Desenvolvimento

> Referência completa: `docs/diretriz-avanade-inventory.md` (197 arquivos categorizados)
> Fonte: `Diretriz Avanade/` (docs oficiais Avanade BCA v3)

---

## 1. BACKEND (C# / Plugins Dataverse)

### Padrão Obrigatório: Plugin → Service → Repository
- Plugins DEVEM herdar de `Plugin<T>` (genérico) ou `Plugin` (Delete/RetrieveMultiple)
- Business logic NUNCA no plugin — sempre em Services separados via interface
- Acesso a dados via `IRepository<T>` (padrão Repository)
- DI via Unity container — auto-registro por convenção: `IFoo` → `Foo`

### Registro de Plugins
- SEMPRE via `[PluginRegistration]` attribute (registro automático APRT)
- NUNCA registrar manualmente via Plugin Registration Tool
- Propriedades: `EntityLogicalName`, `Stage`, `MessageName`, `FilteringAttributes`, `AsAsync`, `IncludePreImage`

### Early Bound Entities — OBRIGATÓRIO
- Gerar via CLI `GenerateEarlyBound` (backend, frontend ou ambos)
- Performance é idêntica a Late Bound (<1% diferença comprovada por benchmark)
- Ganhos: IntelliSense, type safety, compile-time verification, manutenibilidade

### Custom APIs
- Usar `CustomApiPlugin<TRequest, TResponse>` com `[DataMember]` para I/O automático
- Commands (Create/Read de entidades) como alternativa leve para comunicação frontend→backend

### Extension Services (reutilizar antes de reimplementar)
- `FetchXmlService` — avaliação de regras em memória
- `EmailService` — envio com templates + Dynamic Expressions
- `DynamicExpressionService` — resolve campos multi-nível relacionados
- `SecurityRuleService` — atribuição de owner teams/BU por regras
- `AzureServiceBusService` — mensagens otimizadas a partir de plugins
- `AzureADTokenService` — OAuth tokens para Azure Functions/APIs
- `MetadataService` — cache de metadata (primary ID, field info)
- `TranslationService` — `.resx` no backend
- `TimeZoneService` — conversão local↔UTC com cache

### Elevação de Privilégio
- Plugin inteiro: `AsAdmin = true` no `[PluginRegistration]`
- Por query: `.AsAdmin<T>()` no repository

### Contexto e Detecção de Mudanças
- Usar `OperationContext` (stage-aware) em vez de `IPluginExecutionContext`
- `AttributeChangedService<T>`: `HasChanged`, `HasChangedTo`, `GetPreviousValue`
- Target + Pre-Image já são merged automaticamente pelo BCA

### Logging
- Configurável via entidade `Logging Configuration`
- Suporta: CRMTracingService, WindowsDebugOutput, Azure Application Insights

---

## 2. FRONTEND (TypeScript / Form Scripting)

### TypeScript — OBRIGATÓRIO
- TODO código frontend em TypeScript, transpilado via Babel/Webpack
- Registrar entrypoints em `bundle.config.js`
- Target browsers em `babel.config.js`
- Output em `dist/`

### Padrão Obrigatório: Contract / Controller
- Form scripts: `[entity].[form].form.[contract|controller].ts` — Contract é singleton com entry points (onLoad/onSave), Controller contém lógica
- Ribbon scripts: `[entity].ribbon.[contract|controller].ts` — Contract despacha via switch/commandId

### Early Bound Forms e Entities — OBRIGATÓRIO
- Gerar via `config.json` + `GenerateFrontend.ps1`
- Detecta breaking changes em compile time

### Fluent Rules (preferência sobre event handlers manuais)
- `ValueRule`, `EnableRule`, `DisplayRule`, `RequiredRule`
- Reavaliação automática por dependências entre atributos

### OData Query Builder
- API fluente tipo LINQ: `.for(Entity).select().where().orderBy().execute()`
- Early Bound com IntelliSense completo

### Caching — ESSENCIAL
- Dataverse Web API retorna `no-cache` — implementar `DefaultCache` + `CacheItem`
- Usar hash SHA-256 do request como chave

### Coding Standards
- `===` sempre (nunca `==`)
- JSON (nunca XML) para dados
- Blocos single-line com brackets
- New-line após blocks
- Verificar métodos existentes antes de criar novos

---

## 3. TESTING

### Princípio: Shift-Left
- Testar o mais cedo possível — preferir unit tests sem ambiente Dataverse
- Categorias: Unit Tests → Integration Tests → UI Automation Tests

### Backend: MSTest + NSubstitute
- Padrão AAA (Arrange, Act, Assert)
- Naming: `[Class]_Method_Should_[Behavior]_When_[State]`
- Mock via `Substitute.For<T>()`

### Frontend: Jest + Should.js
- `DynamicsContextMock` para simular formulários
- Debug via VS Code: Jest All, Jest Current File, Jest Watch

### Code Coverage
- MANDATÓRIO acima do threshold acordado
- Cada dev responsável por manter/aumentar

### Análise Estática
- Mínimo: Microsoft Managed Recommended Rules
- Recomendado: StyleCop (C#), ESLint (TS)
- Dinâmica: SonarCloud/SonarQube

---

## 4. PIPELINES (4 YAML Pipelines)

| Pipeline | Trigger | Função |
|----------|---------|--------|
| `build-guard` | Automático (CI) | Build frontend+backend + executa TODOS unit tests |
| `deploy-code` | Automático (PR merge) | Deploy artifacts para Customization Master |
| `deploy` | **MANUAL** | Full deploy para upper environments (solutions + data + pre/post steps) |
| `export-unpack` | Opcional | Exporta/desempacota solutions → cria PR automático |

### Regras de Pipeline
- Deploy pipeline é SEMPRE manual — nunca automatizar deploy para upper environments
- Guard build DEVE passar antes de qualquer PR merge
- Secrets via Key Vault linked Library (NUNCA no repositório)
- Placeholders `{SecretVariable}` em CSV substituídos automaticamente

---

## 5. TOOLS (DPA + EMC)

### DPA (Deployment Tool)
- Automação de deployment tasks via XML commands ou PowerShell Cmdlets
- Pre/PostSteps.xml para automação de deployment
- Testável localmente via CLI `ExecuteDPA` antes de commit

### EMC (Entity Management Cockpit)
- Import/export de dados via Excel/CSV/XML
- Config data (todos envs), Test data (Dev-Test), Other data
- Superior ao Configuration Migration Tool da Microsoft

### CLI (`bizapps-cli.ps1`)
- `NewSolution`, `GenerateEarlyBound`, `LocalDeployment`, `UnpackSolution`
- `TransportSolution`, `ReInitializeLowerEnvironment`, `SetupLocalDevEnvironment`
- `ExportImportData`, `ExecuteDPA`, `InstallTools`

---

## 6. FLUXO DE TRABALHO DIÁRIO

1. **Develop**: VS pull latest → generate early bound → develop → tests + coverage
2. **Deploy local**: CLI `LocalDeployment Backend|Frontend` → verify
3. **Customize**: make.powerapps.com → Customization Master → modificar
4. **Unpack**: CLI `UnpackSolution` → review changes
5. **Commit**: Feature branch → commit com `#workitem`
6. **PR**: Título descritivo, work item linkado, reviewers
7. **CI**: Guard build auto-trigger (build + tests)
8. **Deploy**: Pipeline manual para upper environments

---

## 7. ARCHITECTURE & REPO

### Single Git Repo = Single Source of Truth
- Estrutura: `cicd/`, `cli/`, `src/` (Backend, Data, Deployment, Solutions, Web)
- `Configuration.json`: publisher, solutions, environments, parameters
- Customization Master → unpack → commit → PR → deploy

### Solution Segmentation
- Planejada ANTES de começar (base + funcional)
- Managed default para upper environments
- Brownfield: `pac solution clone` para integrar solutions existentes

### Branch Policies (obrigatórias)
- Mínimo 1 reviewer
- Linked work items obrigatórios
- Guard build como pré-requisito de PR
- Comment resolution required

### Environment Approvals
- Exclusive lock (evitar deploys concorrentes)
- Branch control (refs/heads/main)
- Business hours para PROD

---

## 8. CODE REVIEW CHECKLIST

- [ ] Git: branches limpas, push frequente
- [ ] Ambientes dev: componentes removidos após uso
- [ ] Coding: formatação, naming, constantes (não magic numbers)
- [ ] C#: zero warnings, StyleCop rules
- [ ] TypeScript: ESLint clean, `===` sempre
- [ ] Unit tests: AAA, naming convention, coverage ≥ threshold
- [ ] Performance: sem roundtrips desnecessários, caching aplicado

---

## 9. LICENSING (importante para contratos)

- **SDK** (npm frontend + NuGet backend) → entrelaçado com business logic → ENTREGUE ao cliente
- **Tools** (DPA, EMC, Pipelines) → IP Avanade → NÃO são hand-over padrão
- **Eject Script** → automatiza leave-behind (economiza 80-120h)
- Libs 3rd party: aprovadas via Black Duck scan
- PCF third-party: requer OSS scan formal (~$1000)

---

## 10. PERFORMANCE

- DI container: ~150ms apenas na PRIMEIRA invocação (desprezível)
- Early vs Late Bound: <1% diferença — Early Bound SEMPRE
- Evitar roundtrips ao servidor, bundle OData requests
- Não atualizar IFRAMEs redundantemente
- Preferir classes estáticas, evitar nested loops
- Cachear stable data no frontend
