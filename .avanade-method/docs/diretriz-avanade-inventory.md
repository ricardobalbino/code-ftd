# Inventário Completo — Diretriz Avanade BCA (BizApps Core Accelerator)

> **197 arquivos .md** lidos e categorizados  
> Data: 17/Mar/2026

---

## 🔷 BACKEND (30 arquivos)

| # | Caminho relativo | Resumo |
|---|-----------------|--------|
| 1 | development/Backend/index.md | Index page da seção backend. Contém apenas uma frase introdutória sobre guidelines de backend code. |
| 2 | development/Backend/Early-Bound-Entity-Classes.md | Geração de Early Bound Entities (.NET/C#) via CrmSvcUtil.exe customizado. Localização em `Dataverse.Common\Entities.Generated`, uso de Configuration.json para conexão e suporte a login interativo OAuth. |
| 3 | development/Backend/Dependency-Injection.md | Explica o DI container (Unity) do BCA. Registra automaticamente por convenção de nomes (IFoo→Foo). Resolve o grafo de dependências inteiro com uma única chamada a `Resolve`. |
| 4 | development/Backend/Debugging-Plugins.md | Configurações de VS para debug de plugins: desabilitar "Just My Code", habilitar Symbol Server (Enterprise), usar Plugin Profiler em Isolation Mode = None. |
| 5 | development/Backend/CustomAPI.md | Como criar plugins para Custom APIs usando `CustomApiPlugin<TRequest, TResponse>`. Extração automática de InputParameters/OutputParameters via reflection e `[DataMember]`. |
| 6 | development/Backend/CRUD-operations.md | Padrão Repository (`IRepository<T>`) para todas operações CRUD. Abstração sobre OrganizationService, promove SRP, testabilidade e separação de concerns. |
| 7 | development/Backend/Commands.md | Conceito de Commands: comunicação frontend→backend bidirecional via Create/Read de entidades. Alternativa leve a Custom APIs, com request/response serializados via `[DataContract]`. |
| 8 | development/Backend/Stage-and-Target-awareness.md | `OperationContext` substitui `IPluginExecutionContext` com métodos stage-aware (Update, Delete, Associate). Protege a lógica de contexto de stage/message. |
| 9 | development/Backend/Execute-As-Admin.md | Elevação de privilégio: `AsAdmin = true` no `[PluginRegistration]` para todo plugin ou `.AsAdmin<T>()` por repository para queries específicas como admin. |
| 10 | development/Backend/Logging.md | Logging configurável via entidade `Logging Configuration`: suporta CRMTracingService, WindowsDebugOutput (on-prem) e Azure Application Insights (com InstrumentationKey). |
| 11 | development/Backend/Plugins/Write-a-Plugin.md | Estrutura de plugin BCA: herdar de `Plugin<T>` (genérico) ou `Plugin` (não-genérico para Delete/RetrieveMultiple). Plugin como anti-corruption layer que despacha para services. |
| 12 | development/Backend/Plugins/Services-aka-Business-Logic.md | Services encapsulam business logic como first-class citizens do domínio. Usam DI, repositories e atuam como facade pattern para lógica de aplicação. |
| 13 | development/Backend/Plugins/Register-Plugins.md | Registro automático de plugins via `[PluginRegistration]` attribute com reflection (APRT). Propriedades: EntityLogicalName, Stage, MessageName, FilteringAttributes, AsAsync, etc. Deploy via pipeline. |
| 14 | development/Backend/Plugins/Handling-the-target.md | Merge automático de target + pre-image: o BCA combina os dois para fornecer visão completa do registro. Não usa Post-Image pois o merge já representa o estado pós-operação. |
| 15 | development/Backend/Plugins/Detecting-changes.md | `AttributeChangedService<T>` detecta mudanças comparando target vs pre-image. Métodos: `HasChanged`, `HasChangedTo`, `HasChangedFrom`, `GetPreviousValue`. Suporta mocking em testes. |
| 16 | development/Backend/Testing/index.md | Placeholder — sem conteúdo. |
| 17 | development/Backend/Testing/Testing-101.md | Estrutura de testes MSTest: `[TestClass]`, `[TestInitialize]`, `[TestMethod]`. Naming: `[StateUnderTest]_[ExpectedBehavior]`. Classes de teste = nome da classe + "Tests". |
| 18 | development/Backend/Testing/Mocking-examples.md | Exemplos com NSubstitute: mock de interfaces, retorno de valores, `Arg.Is`/`Arg.Any`, verificação de chamadas recebidas, verificação de argumentos em chamadas. |
| 19 | development/Backend/Testing/Getting-started.md | Tutorial introdutório: arranjar mock com `Substitute.For<>`, atuar e assertar. Padrão AAA (Arrange, Act, Assert). |
| 20 | development/Backend/Testing/Examples.md | Exemplos completos: mock de `IRepository`, setup via `[TestInitialize]`, verificação de `_repository.Update()` e `_repository.Received()`. |
| 21 | development/Backend/extension-services/index.md | Introdução a Extension Services: serviços pré-construídos que resolvem desafios avançados de business logic, acessíveis via DI. |
| 22 | development/Backend/extension-services/Translation-Service.md | Serviço que permite uso de arquivos `.resx` (web resources) no backend. Métodos para tradução por user language, userId ou languageCode. |
| 23 | development/Backend/extension-services/Time-Zone-Service.md | Conversão local↔UTC, detecção de timezone por userId, offset calculation e UI name. Cache inteligente para reduzir overhead. |
| 24 | development/Backend/extension-services/Security-Rule-Service.md | Engine de segurança: atribui owner teams ou owning business unit baseado em FetchXML rules ou registro pai. Suporta modernize business units (2022+). |
| 25 | development/Backend/extension-services/Metadata-Service.md | Acesso rápido a metadata com cache: primary ID/name attribute, field metadata, verificação de DateOnly/Timezone. |
| 26 | development/Backend/extension-services/Fetch-XML-Service.md | Avaliação eficiente de regras FetchXML em memória contra registros. Converte FetchXML→QueryExpression. Usado em Security Rules, Notification Engine, Routing. |
| 27 | development/Backend/extension-services/Email-Service.md | Envio de emails com/sem templates, resolução de Dynamic Expressions para dados dinâmicos. Também renderiza templates para uso fora de emails (PDF headers etc.). |
| 28 | development/Backend/extension-services/Dynamic-Expression-Service.md | Supera limitações de Merge Fields: resolve campos de tabelas relacionadas multi-nível, converte DateTime p/ timezone alvo. Suporta anotações via atributo `[ResolveFromDynamicExpression]`. |
| 29 | development/Backend/extension-services/Azure-Service-Bus-Service.md | Envio otimizado de mensagens via Service Bus/Webhook a partir de plugins. Usa Dynamic Expressions para montar payload com uma única retrieval. |
| 30 | development/Backend/extension-services/Azure-Active-Directory-Token-Service.md | Requisita access tokens OAuth v1/v2 para Azure resources via service principal. Usado para chamadas a Azure Functions ou produção de bearer tokens. |

**Key Takeaway — BACKEND:**  
Toda lógica backend DEVE seguir o padrão Plugin → Service → Repository com DI via Unity container. Plugins herdam de `Plugin<T>` e usam `[PluginRegistration]` para registro automático (nunca manual via Plugin Registration Tool). Business logic NUNCA fica no plugin — sempre em services separados via interface. Early Bound Entities são obrigatórias (performance equivalente a late-bound, com ganhos enormes de manutenibilidade). Os Extension Services (Email, FetchXML, ServiceBus, etc.) devem ser reutilizados antes de reimplementar lógica similar.

---

## 🔷 FRONTEND (18 arquivos)

| # | Caminho relativo | Resumo |
|---|-----------------|--------|
| 1 | development/Frontend/index.md | Index vazio — apenas header "Frontend". |
| 2 | development/Frontend/Typescript.md | Frontend usa TypeScript + Babel.js para transpilação até ES5. Módulos ES2015, variáveis let/const, arrow functions, async/await. Polyfills automáticos baseados em target browsers. |
| 3 | development/Frontend/Early-Bound-Entity-Classes.md | Geração de classes ES2015 para entidades frontend. Config via `config.json` + `GenerateFrontend.ps1`. IntelliSense e detecção de breaking changes. |
| 4 | development/Frontend/Early-Bound-Forms.md | Geração de classes early-bound para forms (atributos, tabs, seções). Configurável por formType/formName no config.json. |
| 5 | development/Frontend/Caching.md | Caching necessário porque Web API Dataverse retorna `no-cache`. `DefaultCache` + `CacheItem` com hash SHA-256 do request. Dois tipos: stable data e mitigação de request duplicados. |
| 6 | development/Frontend/Bundling.md | Webpack + Babel para bundle de TypeScript. Entrypoints em `bundle.config.js`, target browsers em `babel.config.js`. Modo dev (sem transpilação) para debug. Output em `dist/`. |
| 7 | development/Frontend/Fluent-Rules.md | Sistema declarativo de regras para form scripting: ValueRule, EnableRule, DisplayRule, RequiredRule etc. Dependências entre atributos com reavaliação automática. Substitui event handlers manuais. |
| 8 | development/Frontend/OData-Query-Builder.md | API fluente tipo LINQ para queries OData: `.for(Entity).select().where().orderBy().execute()`. Suporta expand, filter, top, count com Early Bound entities. |
| 9 | development/Frontend/Form-Scripting/index.md | Index vazio. |
| 10 | development/Frontend/Form-Scripting/Writing-Form-Scripts.md | Estrutura contract/controller para form scripts. Contract: singleton + entry points (onLoad/onSave). Controller: lógica de interação com form. Naming: `[entity].[form].form.[contract/controller].ts`. |
| 11 | development/Frontend/Form-Scripting/Register-Form-Scripts.md | Como registrar form scripts no Model Driven App: adicionar library, criar event handler para onLoad/onSave apontando para `[Entity]FormContracts.onLoad`, marcar "pass execution context". |
| 12 | development/Frontend/Ribbon-Scripting/index.md | Index vazio. |
| 13 | development/Frontend/Ribbon-Scripting/Writing-Ribbon-Scripts.md | Estrutura contract/controller para ribbon scripts. Contract despacha via switch/commandId para actions e rules no controller. Naming: `[entity].ribbon.[contract/controller].ts`. |
| 14 | development/Frontend/Ribbon-Scripting/Register-Ribbon-Rules-and-Actions.md | Registro de ribbon buttons via XrmToolBox + Ribbon Workbench. Entry points: `executeAction` e `executeRule`. Parâmetros de identificação de comando via string params. |
| 15 | development/Frontend/Testing/Testing-101.md | Infra de testes frontend com Jest, Babel, coverage. Padrão AAA. Assertions via Should.js. Mocking de objetos complexos simplificado pelo BCA. |
| 16 | development/Frontend/Testing/Writing-JavaScript-Tests.md | Como escrever testes: `describe`/`test`, `DynamicsContextMock`, mock de atributos de form, simulação de operações async. |
| 17 | development/Frontend/Testing/Debugging-Frontend-Tests.md | Debug via VS Code: configs Jest All, Jest Current File, Jest Watch. Launch.json com node debugger. |
| 18 | development/Frontend/Form-Scripting/index.md | (duplicado vazio do #9) |

**Key Takeaway — FRONTEND:**  
Todo código frontend DEVE ser escrito em TypeScript, transpilado via Babel/Webpack e registrado em `bundle.config.js`. Form scripts seguem obrigatoriamente o padrão Contract/Controller (singleton). Early Bound Forms e Entities são mandatórias para IntelliSense e detecção de breaking changes. Fluent Rules são o approach recomendado para form scripting (substitui event handlers manuais). Caching é essencial pois Dataverse Web API não suporta cache HTTP nativo.

---

## 🔷 GUIDELINES (8 arquivos)

| # | Caminho relativo | Resumo |
|---|-----------------|--------|
| 1 | development/Guidelines/index.md | Todos os membros devem usar Azure DevOps, VS/VS Code com source control, seguir layout definido e guidelines publicadas (Customizing Guidelines, DevOps Process Automation). |
| 2 | development/Guidelines/code-review-checklist.md | Checklist detalhado: Git (limpeza de branches, push frequente), ambientes dev (remover componentes), coding (formatação, naming, constantes), C# (warnings), TS (lint), unit tests (AAA, naming, coverage). |
| 3 | development/Guidelines/code-coverage.md | Code coverage DEVE estar acima do threshold acordado. Todo desenvolvedor é responsável por manter/aumentar o threshold a cada desenvolvimento. |
| 4 | development/Guidelines/code-analysis.md | Static analysis: regras "Microsoft Managed Recommended Rules" como mínimo. Recomendado StyleCop para análise adicional. Dynamic analysis via SonarCloud/Qube. |
| 5 | development/Guidelines/testing.md | Shift-left: testar o mais cedo possível. Categorias: Unit Tests (sem ambiente), Integration Tests (endpoints/APIs), UI Automation Tests (browser). Naming: `[Class]_Method_Should_Behavior_When_State`. |
| 6 | development/Guidelines/performance.md | Evitar roundtrips ao servidor, bundle OData requests, aplicar caching, não atualizar IFRAMEs redundantemente. Usar JSON (não XML), `===` em JS, evitar nested loops, preferir classes estáticas. |
| 7 | development/Guidelines/high-quality-code.md | Cultura de qualidade: notificar tech lead sobre código subótimo, gaps em processos, documentação faltante, trabalho manual que deveria ser automatizado. |
| 8 | development/Guidelines/dataverse-development.md | Standards de desenvolvimento Dataverse: Web Resources via pasta `web_resources`, TypeScript obrigatório, check métodos existentes antes de criar novos, blocos single-line com brackets, `===` sempre, new-line após blocks. |

**Key Takeaway — GUIDELINES:**  
Code reviews são obrigatórios com checklist formal (Git, coding standards, unit tests). Testes seguem Shift-Left: preferir unit tests rápidos sobre Integration/UI tests. Naming de testes é rígido: `Class_Method_Should_Behavior_When_State`. Performance: evitar roundtrips, cachear stable data, usar `===` e JSON. Análise estática com Managed Recommended Rules + StyleCop é padrão mínimo. Todo código deve ter coverage acima do threshold acordado.

---

## 🔷 PIPELINE (8 arquivos)

| # | Caminho relativo | Resumo |
|---|-----------------|--------|
| 1 | Pipelines/index.md | Overview: 4 pipelines YAML — build-guard (CI), deploy-code (para Customization Master), export-unpack (auto PR), deploy (full deploy upper envs). |
| 2 | Pipelines/dataverse-build-guard.md | Pipeline CI trigger: build frontend + backend + executa todos unit tests a cada checkin. |
| 3 | Pipelines/dataverse-deploy-code.md | Build artifacts, executar testes, deploy de webresources e plugins para Customization Master (para associar a solutions). |
| 4 | Pipelines/dataverse-deploy.md | Pipeline completo: build → prepare solutions → por ambiente: import managed solutions, pre-steps, import all solutions, import data, post-steps, publish. |
| 5 | Pipelines/dataverse-export-unpack.md | Pipeline opcional: exporta/desempacota solutions do Customization Master e cria PR automaticamente (alternativa ao CLI local). |
| 6 | Pipelines/bca-vs-build-tools.md | Comparação BCA vs Microsoft Power Platform Build Tools: BCA vence em setup sem extensão AzDO, Key Vault, cert auth, uso local+pipeline, data import avançado, deployment tasks. |
| 7 | Pipelines/Self-Hosted-Agent-Setup.md | Setup de agentes self-hosted: hardware, configuração de PAT, Agent Pool, proxy handling (environment variables), instruções passo-a-passo. |
| 8 | Pipelines/performance-improvements.md | Otimizações: self-hosted agents, parallel jobs, skip test execution em deploy (se guard build garantido), cache de packages. |

**Key Takeaway — PIPELINE:**  
4 pipelines YAML são o core: Guard (CI em cada push), Deploy Code (artifacts para Customization Master), Deploy (full deploy upper envs com data + pre/post steps) e Export-Unpack (opcional, PR automático). Pipeline de deploy é trigger MANUAL para controle total. BCA supera Microsoft Build Tools em flexibilidade (Key Vault, cert auth, ferramentas locais). Para performance, considerar self-hosted agents e skip de testes duplicados no deploy.

---

## 🔷 TOOLS (29 arquivos)

| # | Caminho relativo | Resumo |
|---|-----------------|--------|
| 1 | tools/index.md | Index — introdução à seção de ferramentas do BCA. |
| 2 | tools/Early-Bound-Generator.md | Extensão do CrmSvcUtil: entidades em arquivos separados, CamelCase, filtro por regex, exclusão de atributos, base classes customizadas, split files. Configuração via `CrmSvcUtil.exe.config`. |
| 3 | tools/Deployment-Tool/index.md | DPA (Deployment Tool): ferramenta de deployment via XML commands ou PowerShell Cmdlets. Changelog de releases (3.0.12 até 1.13.0). |
| 4 | tools/Deployment-Tool/quick-start-guide.md | Quick start: install via CLI, Approach A (command-line com XML), Approach B (PowerShell Cmdlets com `Import-Module` + `Connect-Dataverse`). |
| 5 | tools/Deployment-Tool/XML-Configuration.md | Config XML do DPA: parâmetros estáticos, piped, dinâmicos, results. Command-line syntax com switches para override. |
| 6 | tools/Deployment-Tool/PowerShell-Cmdlets.md | Uso via PowerShell: `Import-Module`, `Connect-Dataverse`, `Get-SolutionVersion`. Requer Windows PowerShell (não Core). |
| 7 | tools/Deployment-Tool/Contribution.md | Como criar novos Commands no DPA: adicionar ao CommandGroup.cs, criar classe Command com parâmetros, implementar lógica. |
| 8 | tools/Deployment-Tool/Common-Use-Cases/index.md | Index vazio. |
| 9 | tools/Deployment-Tool/Common-Use-Cases/Set-Dataverse-Application-User.md | Automação de criação de Application User no AD + Dataverse via XML ou PowerShell script. |
| 10 | tools/Deployment-Tool/Common-Use-Cases/Create-Sprint-Plan-from-Azure-DevOps.md | Geração de Sprint Plan (markdown/HTML) a partir de Azure DevOps work items via DPA com PAT. |
| 11 | tools/Deployment-Tool/Common-Use-Cases/automate-dataverse-tasks-with-cmdlets.md | Tutorial detalhado: setup DPA para cmdlets locais, conectar via Configuration.json, exemplo de export de solutions via script PowerShell. |
| 12 | tools/Entity-Management-Cockpit/index.md | EMC: ferramenta de import/export de dados via Excel/CSV/XML. Changelog de releases. Target: devs, testers, rollout coordinators. |
| 13 | tools/Entity-Management-Cockpit/quick-start-guide.md | Six-Step Setup: configurar connection, rodar CRMEMC.App.exe, carregar template, selecionar entidades, importar. Cenários: test data, rollout global, config centralizada. |
| 14 | tools/Entity-Management-Cockpit/Comparison-with-Configuration-Migration-Tool.md | Tabela comparativa EMC vs Configuration Migration Tool (Microsoft). EMC: superior em bulk operations, command line, assignments, clean-up, advanced find queries. |
| 15 | tools/Entity-Management-Cockpit/Configuration/index.md | Index da configuração — apenas header. |
| 16 | tools/Entity-Management-Cockpit/Configuration/app.config.md | Tabela de settings do app.config: ConnectionString, CacheSize, proxy, language codes, string replacements, flags de importação. |
| 17 | tools/Entity-Management-Cockpit/Configuration/entity.md | Config de entidades XML: name, sheet, key, keycolumn, startrow, action (delete), ignoreselfreferencing. |
| 18 | tools/Entity-Management-Cockpit/Configuration/column.md | Config de colunas XML: 20+ tipos (decimal, datetime, optionset, fetchxml, securityrole, merge, guid, file, etc.), referências, default values, concatenação, dynamic expressions. |
| 19 | tools/Entity-Management-Cockpit/Configuration/logging.md | NLog config: logfile, logfile_error_min, logfile_fatal, logfile_error, console targets. |
| 20-29 | tools/Entity-Management-Cockpit/Common-Use-Cases/*.md | 13 arquivos de use cases: Duplicate Detection Rules, Document Templates, Default Values/Concatenation, Data Deletion, Combined Key Lookup, Bulk Activation/Deactivation, Attachments, Force Create, Export, Email Templates, Dynamic Expressions, Import XML Files, Import/Export All CSVs, Merge Entities, Saved Views, Resolve Case/Close Opportunity, N:M Relationships. |
| 30+ | tools/Entity-Management-Cockpit/Scenarios/*.md | 12 cenários: Master Data Management, Case Export/Import, Business Unit Structure, MultiSelect Migration, Portal Config, Field Service Config, Currency Import, Linked Column Export, Auto XML Schema, Close Opportunities, Unified Routing, User Management. |

**Key Takeaway — TOOLS:**  
DPA (Deployment Tool) e EMC (Entity Management Cockpit) são as duas ferramentas principais. DPA automatiza deployment tasks (system settings, security roles, solution imports) via XML ou PowerShell Cmdlets. EMC gerencia import/export de dados (config data, test data, rollout) superando o Configuration Migration Tool da Microsoft em funcionalidades avançadas. Ambas são acessíveis via `bizapps-cli.ps1` e integradas aos pipelines. Para leave-behind, o Eject Script move todo o código dessas ferramentas para o repositório do projeto.

---

## 🔷 PCF (1 arquivo)

| # | Caminho relativo | Resumo |
|---|-----------------|--------|
| 1 | development/PCF/index.md | Guidelines PCF: verificar pcf.gallery antes de criar do zero. Usar `pac pcf init`, `npm install`, `npm run build`, `pac solution init`. OSS scan obrigatório (~$1000) para componentes de terceiros. |

**Key Takeaway — PCF:**  
Antes de desenvolver PCF do zero, SEMPRE verificar pcf.gallery para componentes existentes. Uso de third-party controls requer formal Open Source Software scan (~$1000). Seguir CLI commands padrão Microsoft para init/build/deploy. PCF é adicionado via `bizapps-cli.ps1` → `NewPcfComponent`.

---

## 🔷 ARCHITECTURE (5 arquivos)

| # | Caminho relativo | Resumo |
|---|-----------------|--------|
| 1 | getting-started/overview/overall-structure.md | Estrutura completa do repositório: cicd/, cli/, src/ (Dataverse/Backend, Data, Deployment, Solutions, Web), Configuration.json. Tudo em um único Git repo. |
| 2 | getting-started/overview/configuration-json.md | Configuration.json: Publisher, Solutions (com Plugins/Webresources), ManagedSolutions, Environments (connection strings com placeholders), GlobalParameters e Parameters por ambiente. |
| 3 | getting-started/overview/solution-flows.md | Fluxo: Customization Master → unpack → commit → PR → pipeline auto-deploy. Steps: plan segmentation, create solutions, develop code, modify customizations, deploy via YAML. |
| 4 | getting-started/overview/pipelines.md | Overview curto apontando para seção Pipelines detalhada. YAML pipelines = pipeline as code, per-branch. |
| 5 | getting-started/overview/light-weight.md | BCA suporta light-weight mode (no-code/low-code) com deploy automático. Fácil transição para pro-code adicionando backend/frontend. |

**Key Takeaway — ARCHITECTURE:**  
O repositório Git é a single source of truth: customizações, código, dados e automação. Configuration.json é o arquivo central de configuração (publisher, solutions, environments, parameters). O fluxo padrão é: customizar em Customization Master → unpack via CLI → commit → PR → deploy automático. BCA suporta desde light-weight (no-code) até pro-code no mesmo repositório e estrutura.

---

## 🔷 TESTING (3 arquivos — adicionalmente aos de Backend/Frontend Testing acima)

| # | Caminho relativo | Resumo |
|---|-----------------|--------|
| 1 | development/Guidelines/testing.md | Princípios: shift-left, 3 categorias (Unit, Integration, UI Automation). Naming: `Class_Method_Should_Behavior_When_State`. Test data ref a data-management. |
| 2 | development/Guidelines/code-coverage.md | Code coverage acima do threshold acordado é MANDATÓRIO. Cada dev deve contribuir para aumentar. |
| 3 | development/Guidelines/code-analysis.md | Análise estática: Microsoft Managed Recommended Rules (mínimo), StyleCop (recomendado), SonarCloud (dinâmica). |

**Key Takeaway — TESTING:**  
Shift-left é o princípio fundamental: testar lógica em unit tests rápidos sem precisar de ambiente Dataverse. NSubstitute (backend) e Jest (frontend) são os frameworks de mocking/testing. Naming de testes DEVE seguir `Class_Method_Should_Behavior_When_State`. Code coverage acima do threshold é responsabilidade individual de cada desenvolvedor. Testes rodam tanto no Guard Build (CI) quanto no Deploy Pipeline.

---

## 🔷 SETUP (16 arquivos)

| # | Caminho relativo | Resumo |
|---|-----------------|--------|
| 1 | getting-started/index.md | Introdução ao BCA: set de libraries, tools, templates para produtividade com Dataverse. Áreas: Customizing/Dev Automation, Backend Dev, Frontend Dev. |
| 2 | getting-started/bigger-picture.md | Componentes: BizApps Platform (web UI), Project Repository (Git), Pipelines (YAML), Solution Segmentation, CLI, Configuration.json, NuGet Feed, Environments (Dev, CM, Dev-Int, Dev-Test, UAT, Prod). |
| 3 | getting-started/first-steps/create-engagement.md | Criar engagement via BizApps Platform (platform.avanade.com). Wizard guia o setup completo. |
| 4 | getting-started/first-steps/create-solutions.md | Criação de solutions: base + funcional. Via CLI `NewSolution`. Suporta brownfield (clone existing). Solution segmentation planejada antes. |
| 5 | getting-started/first-steps/installing-tools.md | Pré-requisitos: VS Code, VS 2022, .NET 4.6.2/4.7.2. Tools: Git, NodeJS, Azure Artifacts Credential Provider, Fiddler, PAC CLI, VS Extension. Install automático via `bizapps-cli.ps1 → SetupLocalDevEnvironment`. |
| 6 | getting-started/first-steps/whats-next.md | Próximos passos após setup: adicionar código (backend/frontend/PCF), config/test data, automation steps, cenários avançados. |
| 7 | getting-started/backend/visual-studio-solution.md | Estrutura VS Solution: base projects (Tools, Config), backend (Common, Data, BusinessLogic, Plugins), extension (Commands, Workflows). Cada projeto tem test project correspondente. |
| 8 | getting-started/backend/plugin-classes.md | Comparação Plugin BCA vs SDK padrão. `Plugin<T>` (genérico) vs `Plugin` (não-genérico). `[PluginRegistration]` attribute com propriedades EntityLogicalName, Stage, MessageName, IncludePreImage, Mode. |
| 9 | getting-started/backend/business-logic.md | Referência curta para seção de business logic services. |
| 10 | getting-started/backend/dependency-container.md | Basics do DI container: `container.Resolve<T>()` para SDK types e custom services. Auto-registro por convenção IFoo→Foo. Common mistakes: naming mismatch, interface faltando. |
| 11 | getting-started/backend/understanding-the-example-code.md | Exemplos incluídos: 6 plugins (Create/Update/Delete/RetrieveMultiple + CustomAPI), UnsubscribeCommand, MyCustomAPI. |
| 12 | getting-started/frontend/frontend-structure.md | Estrutura Web em VS Code: config files (.eslintrc, package.json, babel/bundle/tsconfig), src/ (entity, generated, models, webresources), tests/. |
| 13 | getting-started/frontend/understanding-the-example-code.md | Exemplos frontend: form scripts (account standard, contact FluentRules), ribbon scripts, webresource HTML/TS, React example, unit tests. |
| 14 | development/Setup-development-machine.md | OBSOLETO — substituído por installing-tools. Listava VS 2019+, .NET 4.6.2, PowerShell execution policy, VS Code, git, npm, VS Extension. |
| 15 | development/Visual-Studio-Extension.md | VS Extension com templates: Plugin, Plugin-Tests, Workflow, Repository, Form-Script, Ribbon-Script, FluentRules. Wizard para entity/message. Install via CLI ou manual via VSIX. |
| 16 | development/setup-prettier.md | Setup Prettier: criar `.prettierrc`, instalar extensão VS Code. Config: trailingComma es5, tabWidth 4, semi true, singleQuote false, printWidth 140. |

**Key Takeaway — SETUP:**  
O setup é feito via BizApps Platform (engagement) + `bizapps-cli.ps1` (local). Ferramentas obrigatórias: VS Code + VS 2022, Git, NodeJS, PAC CLI, Azure Artifacts Credential Provider. A VS Extension fornece templates para acelerar criação de plugins/form scripts. Solution segmentation DEVE ser planejada antes de começar (base + funcional). Configuration.json é o arquivo central que conecta tudo (environments, solutions, parameters).

---

## 🔷 TROUBLESHOOTING (9 arquivos)

| # | Caminho relativo | Resumo |
|---|-----------------|--------|
| 1 | Troubleshooting/index.md | Index: known issues causados por mudanças na Power Platform ou setup de workstation. Criar support ticket se não encontrar solução. |
| 2 | Troubleshooting/ws-trust-authentication-deprecated.md | Erro de WS-Trust post CW40: mudar ConnectionString para formato OAuth com AppId/RedirectUri padrão. |
| 3 | Troubleshooting/VSTest-task-not-run-in-pipeline.md | VSTest não executa testes: ajustar testAssemblyVer2 pattern, desabilitar codeCoverage e diagnostics, remover runSettingsFile. |
| 4 | Troubleshooting/Restore-and-Rebuild-not-working.md | NuGet restore falha 401: verificar licença MSDN (mínimo Professional), acessar innersource.visualstudio.com. |
| 5 | Troubleshooting/repository-not-visible.md | Repos não visível em AzDO: licença VS Professional ou superior faltando/não vinculada à conta. |
| 6 | Troubleshooting/path-too-long.md | Path longo impede load de projetos: usar `subst x: .` para criar hard link com drive curto. |
| 7 | Troubleshooting/package-not-found.md | Package não encontrado: versão beta removida do feed. Atualizar para versão existente e planejar updates regulares. |
| 8 | Troubleshooting/nuget-pipeline-failing.md | Pipeline 401: PAT expirado para BCA Azure DevOps Artifact. Renovar e atualizar variável `INNERSOURCE_PAT`. |
| 9 | Troubleshooting/npm-authentication-issue.md | npm auth falha (Accenture IDs típico): instalar `vsts-npm-auth -g`, executar `vsts-npm-auth -F -config .npmrc`. |
| 10 | Troubleshooting/Build-Agent-Connection-Issues.md | Agent não conecta: configurar proxy via `System.Net.WebProxy` em scripts PS ou Environment Variables no servidor. Restart necessário. |

**Key Takeaway — TROUBLESHOOTING:**  
Os problemas mais comuns são: autenticação (WS-Trust deprecado → usar OAuth, PAT expirado, npm auth falha), licenciamento (VS Professional mínimo necessário), e infra (path longo, proxy de build agent). Primeiro passo é sempre verificar a seção de troubleshooting antes de abrir ticket. PATs devem ser renovados proativamente. Para ambientes com proxy, configurar tanto em script quanto em environment variables.

---

## 🔷 LICENSING (2 arquivos)

| # | Caminho relativo | Resumo |
|---|-----------------|--------|
| 1 | Licensing/index.md | SDK (frontend npm + backend NuGet) fica entrelaçado com business logic e é entregue ao cliente. Tools e CI/CD pipeline são aceleradores da Avanade e NÃO são parte do hand-over padrão. Backend NuGet = assemblies compiladas (cliente usa mas não pode modificar SDK). Frontend npm = código fonte completo entregue. |
| 2 | Licensing/3rd-Party-Licenses.md | Lista de todas libs 3rd party aprovadas (Black Duck + Security review): Frontend (babel, jest, eslint, webpack, sinon, should, etc.) e Backend (não listado detalhadamente). |

**Key Takeaway — LICENSING:**  
A distinção SDK vs Tools é crítica para contratos. SDK (npm/NuGet) é entrelaçado com business logic e vai ao cliente. Tools (DPA, EMC, pipelines) são IP da Avanade e NÃO são hand-over padrão — exceto via Eject Script que automatiza o leave-behind. Clarificar em contrato que ferramentas de engagement não fazem parte do pacote entregue. Libs 3rd party passaram por Black Duck scan.

---

## 🔷 OTHER (2 arquivos)

| # | Caminho relativo | Resumo |
|---|-----------------|--------|
| 1 | Other/Performance.md | Performance do BCA: setup do DI container toma ~150ms apenas NO PRIMEIRO invocation (per app domain). Invocações subsequentes sem impacto mensurável. |
| 2 | Other/Early-Vs-Late-Bound.md | Benchmark: Early Bound vs Late Bound são virtualmente idênticos em performance (<1% diferença). Early Bound é superior em manutenibilidade e qualidade — não há razão para usar late-bound. |

**Key Takeaway — OTHER:**  
O argumento "late-bound é mais performático" é mito comprovado por benchmark: diferença <1%. Early Bound é SEMPRE a escolha correta (IntelliSense, type safety, compile-time verification). O overhead do DI container BCA é ~150ms apenas na primeira invocação — desprezível para operações Dataverse.

---

## 🔷 HOW-TOS (14 arquivos)

| # | Caminho relativo | Resumo |
|---|-----------------|--------|
| 1 | How-Tos/index.md | Index: "A Day in the Life and Beyond" — How-tos para tarefas com tooling BCA. |
| 2 | How-Tos/new-solution.md | Via CLI: `NewSolution` → publisher → display name → opção de plugin → PR. Usa pac.exe para criar blank solution. |
| 3 | How-Tos/modify-deploy-customizations.md | Fluxo principal: make.powerapps.com → Customization Master → modificar → CLI `UnpackSolution` → review → feature branch → commit → PR → deploy pipeline. |
| 4 | How-Tos/manage-data.md | Data management via EMC: config data (all envs), test data (Dev-Test), other data. Import/export via CLI `ExportImportData`. CSV padrão para tracking de mudanças. |
| 5 | How-Tos/generate-early-bound.md | CLI: `GenerateEarlyBound` → opções backend, frontend ou ambos. |
| 6 | How-Tos/develop-deploy-frontend.md | Fluxo frontend: VS Code → pull latest → generate early bound → develop → test → CLI `LocalDeployment Frontend` → verify → PR. Para novos webresources: aguardar deploy para CM, adicionar a solution, registrar onLoad. |
| 7 | How-Tos/develop-deploy-backend.md | Fluxo plugin: VS pull latest → generate early bound → develop → tests + coverage → CLI `LocalDeployment Backend` → verify → PR. Para novo step: aguardar deploy CM → adicionar step à solution → unpack → PR. |
| 8 | How-Tos/create-pull-request.md | PR em AzDO: título descritivo, work item linkado (#12345), reviewers, approval, complete. Deploy Code auto-trigger para code changes. Deploy pipeline é manual. |
| 9 | How-Tos/automate-pre-post-steps.md | Pre/PostSteps.xml no Deployment folder. Testar localmente via CLI `ExecuteDPA`. Após acerto, commit e auto-deploy em upper environments. |
| 10 | How-Tos/run-deploy-pipeline.md | Pipeline deploy é MANUAL: AzDO → Pipelines → Dataverse Deploy → opcional Upgrade solutions → Run → selecionar solutions. |
| 11 | How-Tos/Others/manage-npm-packages.md | Manutenção npm: `npm outdated`, `npm update`, `npm audit fix`. Tool `ncu` para UX melhor. BCA SDK update via `@avanade/bizapps-sdk`. |
| 12 | How-Tos/Others/manage-lower-environments.md | CLI: `TransportSolution` para importar em dev. `ReInitializeLowerEnvironment` para setup completo (deploy all solutions + data + publisher). |
| 13 | How-Tos/Others/manage-local-dev-environment.md | CLI: `SetupLocalDevEnvironment` para instalar tools. `InstallTools` para baixar EMC/DPA localmente. |
| 14 | How-Tos/Others/develop-deploy-pcf.md | CLI: `NewPcfComponent` → develop com `npm start` → `pac pcf push` → commit → unpack form changes → PR. |
| 15 | How-Tos/Others/administer-project-repository.md | Administração: NuGet/npm package updates, CLI `UpdateCLI` para scripts, `UpdateYAML` para pipelines, Eject Script para leave-behind. |
| 16 | How-Tos/Others/add-secrets.md | Secrets via Key Vault linked Library em AzDO. Placeholders `{SecretVariable}` em CSV substituídos automaticamente. Pipeline config em `dpa-import-data.yaml`. |

**Key Takeaway — HOW-TOS:**  
O fluxo diário é: customizar em Customization Master → unpack via CLI → commit com #workitem → PR → deploy automático. Dados de configuração e teste vivem no repositório (CSV) e são importados automaticamente. Pre/Post steps de deployment são testáveis localmente via CLI antes de commit. Deploy pipeline é SEMPRE manual. Secrets NUNCA ficam no repositório — usar Key Vault + variáveis de pipeline.

---

## 🔷 NEXT-STEPS / SETUP AVANÇADO (16 arquivos)

| # | Caminho relativo | Resumo |
|---|-----------------|--------|
| 1 | getting-started/next-steps/index.md | Header vazio. |
| 2 | getting-started/next-steps/data-management.md | Config data (all envs), Test data (Dev-Test), Other data. CSV como formato padrão. Workflows: editar local→import→commit ou editar em env→export→commit. |
| 3 | getting-started/next-steps/clone-existing-solutions.md | Brownfield: `pac solution clone` → editar .cdsproj com `SolutionPackageType=Both` → build → register em Configuration.json. |
| 4 | getting-started/next-steps/branch-policies.md | Tabela de políticas recomendadas: 1+ reviewer, check linked work items, comment resolution, build validation (guard), merge type consistency. |
| 5 | getting-started/next-steps/environment-approvals.md | AzDO environment approvals: grupo de aprovadores, exclusive lock (evitar collisions), branch control (refs/heads/main), business hours para prod. |
| 6 | getting-started/next-steps/deployment-automation.md | Pre/PostSteps.xml com DPA. Testável localmente via CLI. Biblioteca de exemplos extensiva. |
| 7 | getting-started/next-steps/get-latest-updates.md | Packages: NuGet/npm update. Scripts/YAML: CLI `UpdateCLI` e `UpdateYAML`. |
| 8 | getting-started/next-steps/use-tools.md | DPA e EMC acessíveis via CLI. Install standalone via `InstallTools`. Quick start guides disponíveis. |
| 9 | getting-started/next-steps/translations.md | Translations em managed solutions aumentam tamanho. Opções: pac.exe `-loc` com .resx ou deixar para stage posterior (DevInt). |
| 10 | getting-started/next-steps/secret-handling.md | Versão 3.0: referência para How-Tos/Others/add-secrets.md. Handling genérico em releases futuras. |
| 11 | getting-started/next-steps/run-behind-proxy.md | Proxy: `-Proxy:` parameter em tool calls. Nota: `::` em URLs para distinguir de separadores de parâmetros. |
| 12 | getting-started/next-steps/portal-deployment.md | Power Apps Portals: usar pac.exe para download, armazenar artifacts em repo, deploy via adapted YAML ou pipeline dedicado. |
| 13 | getting-started/next-steps/pcf-development.md | PCF via CLI `NewPcfComponent`. Verificar PCF Library para templates/React. Licensing de third-party requer clearance formal. |
| 14 | getting-started/next-steps/patch-solutions.md | Patch solutions para managed solutions grandes. Suporte completo em próxima versão. Workaround: criar patch manualmente + Configuration.json. |
| 15 | getting-started/next-steps/parallel-developer-flows.md | 1+ Dev instance para 3-5+ developers. Contatar BCA team para issues específicos. |
| 16 | getting-started/next-steps/manual-setup.md | Setup manual (exceção): criar projeto pessoal → BizApps Platform → importar repo para client AzDO → configurar variable groups + pipelines. |
| 17 | getting-started/next-steps/manage-lower-environments.md | Referência para CLI TransportSolution e ReInitializeLowerEnvironment. |
| 18 | getting-started/next-steps/local-feed-setup.md | Setup local feed para eliminar dependência de innersource: npm package local + NuGet packages na pasta feed/. Remover .npmrc. |
| 19 | getting-started/next-steps/leave-behind.md | Eject Script automatiza o leave-behind cortando dependências do feed Avanade. Pós-eject: updates do BCA não são mais automáticos. |

**Key Takeaway — NEXT-STEPS:**  
Branch policies são essenciais: mínimo 1 reviewer, linked work items obrigatórios, guard build como pré-requisito de PR. Environment approvals com exclusive lock evitam deploys concorrentes. Para brownfield, `pac solution clone` integra rapidamente solutions existentes ao BCA. Local feed setup é necessário para ambientes sem acesso ao innersource feed. Leave-behind via Eject Script é completamente automatizado.

---

## 🔷 RELEASE-NOTES (9 arquivos)

| # | Caminho relativo | Resumo |
|---|-----------------|--------|
| 1 | Release-Notes/index.md | V3 objectives: preservar conceitos, abraçar pac.exe, segmentation modular (managed/unmanaged), high-speed + high-quality. Single source of truth no repositório. |
| 2 | Release-Notes/typescript-support.md | Migração completa do frontend BCA para TypeScript. Benefícios: type safety, IntelliSense, debug em VS Code. |
| 3 | Release-Notes/solution-flows.md | Solution flows melhorados: segmentation funcional/técnica, managed default, no-code/low-code/pro-code flexível. |
| 4 | Release-Notes/pcf-support.md | PCF via CLI com setup automatizado: metadata, structure, unit tests skeleton, deploy automático. |
| 5 | Release-Notes/net-core-support.md | Backend package habilitado para .NET Core: Azure Functions, Docker Containers, cross-platform. |
| 6 | Release-Notes/leave-behind.md | Eject Script automatizado: 80-120 horas economizadas por projeto. |
| 7 | Release-Notes/implementation-structure.md | Plugin class abstraction alinhada com CRM SDK padrão. Custom APIs. PluginRegistration attributes para automação. |
| 8 | Release-Notes/extension-services.md | 9 Extension Services com estimativa de horas economizadas: FetchXml (200h), Email (80-100h), DynamicExpression (200-250h), ServiceBus (80-120h), Security (80-120h), etc. |
| 9 | Release-Notes/documentation-improvements.md | Documentação migrada para mkdocs com melhor busca e usabilidade. |

---

## 🔷 SUPPORT (2 arquivos)

| # | Caminho relativo | Resumo |
|---|-----------------|--------|
| 1 | Support/create-support-ticket.md | Passo-a-passo para criar ticket em platform.avanade.com: Support → Create Ticket → Case type → Details + attachment → ticket number. |
| 2 | Support/training.md | Links para treinamento: Level 100 disponível, Level 200 em breve. |

---

## Integrating-External-Services (1 arquivo)

| # | Caminho relativo | Resumo | Tag |
|---|-----------------|--------|-----|
| 1 | development/Integrating-External-Services.md | NuGet `Avanade.CRM.Core.IntegrationLayer` para serviços externos/.NET Console Apps. Mesmo conceito de DI/Repositories/Logging dos plugins. Requer .NET 4.6.2+, namespace "Avanade.*". | BACKEND |

---

## Resumo Quantitativo

| Categoria | Arquivos | % |
|-----------|----------|---|
| BACKEND | 31 | 16% |
| FRONTEND | 18 | 9% |
| GUIDELINES | 8 | 4% |
| PIPELINE | 8 | 4% |
| TOOLS | ~42 | 21% |
| PCF | 1 | 1% |
| ARCHITECTURE | 5 | 3% |
| TESTING | 3 | 2% |
| SETUP | 16 | 8% |
| TROUBLESHOOTING | 10 | 5% |
| LICENSING | 2 | 1% |
| OTHER | 2 | 1% |
| HOW-TOS | 16 | 8% |
| NEXT-STEPS | 19 | 10% |
| RELEASE-NOTES | 9 | 5% |
| SUPPORT | 2 | 1% |
| **TOTAL** | **~197** | **100%** |
