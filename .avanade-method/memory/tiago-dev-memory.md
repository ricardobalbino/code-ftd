### Code Quality Patterns
_Padrões de código que funcionam bem_

**Exemplo**:
```yaml
- pattern: "SOLID Principles"
  S_single_responsibility:
    example: "UserService → UserAuthService + UserProfileService (separados)"
    benefit: "Classes focadas, fáceis de testar"
  O_open_closed:
    example: "Strategy pattern para payment methods (extensível sem modificar existente)"
    benefit: "Adicionar PayPal sem modificar CreditCardPayment"
  L_liskov_substitution:
    example: "Bird → FlyingBird, Penguin (Penguin não herda fly())"
    benefit: "Hierarquias corretas, sem métodos vazios"
  I_interface_segregation:
    example: "IReadable, IWritable separados (não IRepository gigante)"
    benefit: "Clients dependem apenas do que usam"
  D_dependency_inversion:
    example: "Controller → IUserService (interface), não UserService (concreto)"
    benefit: "Testability (mock IUserService), loose coupling"
  
- pattern: "DRY (Don't Repeat Yourself)"
  anti_pattern: "Copy-paste code (duplicação)"
  refactoring: "Extract method, extract class, inheritance/composition"
  caveat: "Não over-DRY (abstrações prematuras → complexidade)"
  rule_of_thumb: "3 duplicações → refactor (rule of three)"
  
- pattern: "YAGNI (You Aren't Gonna Need It)"
  principle: "Não implementar features 'por acaso' (apenas quando needed)"
  example: "Não criar abstração genérica antes de ter 2+ casos de uso"
  benefit: "Código mais simples, menos over-engineering"
  balance: "YAGNI para features, mas plan for extensibility (SOLID-O)"
```

---

### Design Patterns Aplicados
_Padrões GoF e outros_

**Exemplo**:
```yaml
- pattern: "Repository Pattern"
  use_case: "Data access layer (abstração de DB)"
  implementation:
    interface: "IUserRepository { GetById(), Save(), Delete() }"
    concrete: "SqlUserRepository, MongoUserRepository"
  benefit: "Troca de DB transparente, testability (mock repository)"
  projects_used: 15
  
- pattern: "Factory Pattern"
  use_case: "Criação de objetos complexos (payment processors)"
  implementation: "PaymentFactory.Create('creditcard') → CreditCardProcessor"
  benefit: "Centraliza lógica de criação, extensível (adicionar novos tipos)"
  projects_used: 8
  
- pattern: "Observer Pattern (Event-driven)"
  use_case: "Notificações (order placed → send email, update inventory)"
  implementation:
    - "Event: OrderPlacedEvent"
    - "Handlers: EmailNotificationHandler, InventoryUpdateHandler"
  benefit: "Decoupling (order service não conhece email/inventory)"
  projects_used: 12
  
- pattern: "Strategy Pattern"
  use_case: "Algoritmos intercambiáveis (sorting, pricing)"
  implementation:
    - "ISortStrategy { Sort(List<T>) }"
    - "Concrete: BubbleSort, QuickSort, MergeSort"
  benefit: "Swap algorithms runtime, testability"
  projects_used: 6
  
- pattern: "Decorator Pattern"
  use_case: "Add responsibilities dynamically (logging, caching)"
  implementation: "CachedRepository wraps UserRepository"
  benefit: "Composable behaviors, SRP (cada decorator 1 responsabilidade)"
  projects_used: 10
```

---

### Refactoring Techniques Validadas
_Técnicas de refactoring eficazes_

**Exemplo**:
```yaml
- technique: "Extract Method"
  before: "Método de 100 linhas (faz muitas coisas)"
  after: "5 métodos de 20 linhas cada (nomes descritivos)"
  benefit: "Readability +80%, testability (unit test por método)"
  ide_support: "Visual Studio, VS Code (Extract Method refactoring)"
  
- technique: "Replace Conditional with Polymorphism"
  before: |
    if (type == "credit") { ... }
    else if (type == "debit") { ... }
    else if (type == "paypal") { ... }
  after: |
    IPaymentProcessor processor = PaymentFactory.Create(type);
    processor.Process();
  benefit: "Extensível (adicionar tipo sem modificar if/else), SOLID-O"
  
- technique: "Introduce Parameter Object"
  before: "CreateUser(string name, string email, int age, string address, ...)" # 8 params
  after: "CreateUser(UserDto userDto)" # 1 objeto
  benefit: "Readability, evita parameter explosion"
  
- technique: "Replace Magic Numbers with Constants"
  before: "if (status == 3) { ... }"
  after: "if (status == OrderStatus.Shipped) { ... }"
  benefit: "Self-documenting code, typo-safe (enum)"
```

---

### Performance Optimization Patterns
_Otimizações validadas_

**Exemplo**:
```yaml
- optimization: "Caching (Redis)"
  scenario: "User profile lido 1000x/min, muda 1x/dia"
  implementation:
    - "Cache.Get('user:123') → hit 99%, miss 1%"
    - "TTL: 24h (expira e re-fetch de DB)"
  impact: "DB load -99%, API response time 200ms → 15ms"
  caveat: "Cache invalidation complexo (clear on update)"
  
- optimization: "Database Indexing"
  scenario: "Query lenta: SELECT * FROM Orders WHERE UserId = X"
  implementation: "CREATE INDEX idx_orders_userid ON Orders(UserId)"
  impact: "Query time 2s → 50ms (40x faster)"
  caveat: "Índices custam espaço e lentidão em writes (tradeoff)"
  
- optimization: "Async/Await (non-blocking I/O)"
  scenario: "API call externa bloqueia thread (síncrono)"
  implementation: "await httpClient.GetAsync(url) # non-blocking"
  impact: "Throughput +300% (threads liberadas para outros requests)"
  caveat: "Não usar async para CPU-bound (apenas I/O-bound)"
  
- optimization: "Lazy Loading (defer work até necessário)"
  scenario: "Carregar relacionamentos de entidade apenas se usado"
  implementation: "User.Orders (lazy) → load apenas se .Orders acessado"
  impact: "Initial load time -60%"
  caveat: "N+1 query problem (cuidado em loops)"
  
- optimization: "Pagination (limit data transfer)"
  scenario: "Endpoint retorna 10k records → slow, high bandwidth"
  implementation: "GET /users?page=1&pageSize=50"
  impact: "Response time 5s → 200ms, bandwidth -99%"
```

---

## 🐛 Debugging Strategies

### Debugging Techniques Eficazes
_Abordagens sistemáticas de debugging_

**Exemplo**:
```yaml
- technique: "Binary Search Debugging (git bisect)"
  scenario: "Bug introduzido em algum commit dos últimos 50"
  process:
    1. "git bisect start"
    2. "git bisect bad (current commit com bug)"
    3. "git bisect good (commit antigo sem bug)"
    4. "Test commit do meio → mark good/bad"
    5. "Repeat até encontrar commit culpado"
  benefit: "Find bug-introducing commit em log(N) steps (não N)"
  
- technique: "Rubber Duck Debugging"
  process: "Explicar código/problema em voz alta (ou para pato de borracha)"
  benefit: "70% das vezes, solução aparece ao explicar (força raciocínio estruturado)"
  effectiveness: "Alta (baixo custo, alta taxa de sucesso)"
  
- technique: "Breakpoint + Watch Variables"
  tool: "Visual Studio Debugger, Chrome DevTools"
  process:
    1. "Set breakpoint na linha suspeita"
    2. "Run em debug mode"
    3. "Watch variables (inspecionar estado)"
    4. "Step over/into para seguir fluxo"
  benefit: "Visualizar estado runtime (não apenas logs)"
  
- technique: "Logging Estratégico"
  anti_pattern: "Console.WriteLine() em todo lugar"
  pattern: "Structured logging (Serilog, log levels)"
  example: |
    _logger.LogInformation("User {UserId} logged in", userId);
    _logger.LogWarning("API timeout after {Elapsed}ms", elapsed);
  benefit: "Logs agregáveis (Application Insights), filtráveis por level"
  
- technique: "Divide and Conquer (isolar problema)"
  process:
    1. "Reproduzir bug minimalmente (smallest failing test case)"
    2. "Comment out metade do código → still fails?"
    3. "Narrow down até isolar linha problemática"
  benefit: "Reduz search space exponencialmente"
```

---

### Common Error Patterns & Fixes
_Erros recorrentes e soluções_

**Exemplo**:
```yaml
- error: "NullReferenceException (C#) / TypeError (JS)"
  root_cause: "Variável null/undefined não validada"
  fix:
    - "Guard clause: if (user == null) throw ArgumentNullException"
    - "Null-conditional operator: user?.Name (retorna null se user null)"
    - "Nullable reference types (C# 8+): string? (explicit nullable)"
  prevention: "Static analysis (Roslyn analyzers), code review"
  
- error: "Memory Leak (aplicação consume RAM crescentemente)"
  root_cause: "Event handlers não desregistrados, referências circulares"
  fix:
    - "Dispose pattern (IDisposable, using statements)"
    - "Weak references para event listeners (se aplicável)"
    - "Memory profiler (dotMemory, Chrome DevTools) → identify leak"
  prevention: "Code review (check Dispose calls), automated memory tests"
  
- error: "Deadlock (threads bloqueadas mutuamente)"
  root_cause: "Lock ordering inconsistente (Thread A: lock(A) then lock(B), Thread B: lock(B) then lock(A))"
  fix:
    - "Lock ordering global (sempre lock A antes B)"
    - "Evitar locks aninhados (refactor)"
    - "Timeout em locks (Monitor.TryEnter(lock, timeout))"
  detection: "Thread dump analysis (deadlock detector)"
  
- error: "SQL N+1 Problem (performance)"
  root_cause: "Lazy loading em loop (1 query inicial + N queries para relations)"
  fix:
    - "Eager loading: Include(u => u.Orders) em LINQ"
    - "Batch loading (DataLoader pattern em GraphQL)"
  detection: "DB profiler (Entity Framework query logging)"
```

---

## 🏗️ Architecture & Tech Stack Decisions

### Tech Stack Preferences
_Tecnologias validadas em projetos_

**Exemplo**:
```yaml
- category: "Backend Framework"
  preferred: ".NET 8 (C# ASP.NET Core)"
  rationale:
    - "Performance (benchmarks top 3)"
    - "Async/await nativo (I/O efficiency)"
    - "Azure-native (PaaS otimizado)"
    - "Type safety (C# vs JavaScript)"
  alternatives:
    - "Node.js (Express): Usado para APIs simples (CRUD lightweight)"
    - "Python (FastAPI): Usado para ML/data science workloads"
  
- category: "Frontend Framework"
  preferred: "React + TypeScript"
  rationale:
    - "Component-based (reusability)"
    - "TypeScript previne 60% dos bugs (type safety)"
    - "Ecossistema maduro (libraries, tooling)"
  alternatives:
    - "Vue.js: Considerado para projetos menores (learning curve menor)"
  
- category: "ORM (Object-Relational Mapping)"
  preferred: "Entity Framework Core"
  rationale:
    - "LINQ (type-safe queries)"
    - "Migrations (schema versioning)"
    - "Performance adequada (lazy/eager loading control)"
  alternatives:
    - "Dapper: Usado para performance crítica (micro-ORM, mais controle)"
  
- category: "Testing"
  preferred: "xUnit (unit), Playwright (E2E)"
  rationale: "Padrão .NET (xUnit), Playwright multi-browser + auto-wait"
  
- category: "API Design"
  preferred: "REST (OpenAPI/Swagger)"
  rationale: "Simplicidade, tooling (Swagger UI, codegen)"
  alternatives:
    - "GraphQL: Usado quando clients precisam flexibilidade (mobile + web diferentes needs)"
```

---

### Code Review Checklist
_Itens que Tiago valida em PRs_

**Exemplo**:
```yaml
- category: "Functional Correctness"
  checks:
    - "Feature funciona conforme acceptance criteria"
    - "Edge cases cobertos (nulls, empty lists, etc)"
    - "Error handling adequado (try/catch, user-friendly messages)"
  
- category: "Code Quality"
  checks:
    - "SOLID principles respeitados"
    - "Nomes descritivos (sem variáveis x, y, temp)"
    - "Métodos < 20 linhas (exceto casos justificados)"
    - "DRY (sem duplicação excessiva)"
  
- category: "Testing"
  checks:
    - "Unit tests escritos (coverage >80%)"
    - "Tests passando (CI green)"
    - "Integration tests se aplicável (API endpoints)"
  
- category: "Performance"
  checks:
    - "Queries otimizadas (sem N+1)"
    - "Caching considerado (se aplicável)"
    - "Async/await para I/O-bound operations"
  
- category: "Security"
  checks:
    - "Input validation (não confiar em user input)"
    - "SQL injection prevented (parameterized queries)"
    - "Secrets não hardcoded (use environment variables)"
    - "Authentication/authorization corretos"
  
- category: "Documentation"
  checks:
    - "XML comments para public APIs (C#)"
    - "README updated (se mudanças em setup)"
    - "ADR criado (se decisão arquitetural - Wilson)"
```

---

## 🔄 CI/CD & DevOps Practices

### CI/CD Pipeline Configuration
_Setup de pipelines que funciona_

**Exemplo**:
```yaml
- pipeline_stage: "Build"
  tasks:
    - "Restore dependencies (NuGet, npm)"
    - "Compile code (dotnet build)"
    - "Run linters (ESLint, StyleCop)"
  duration: "2-3 minutos"
  failure_action: "Block PR merge"
  
- pipeline_stage: "Test"
  tasks:
    - "Run unit tests (dotnet test)"
    - "Generate coverage report (Coverlet)"
    - "Upload to SonarQube (quality gate)"
  duration: "5-8 minutos"
  failure_action: "Block PR merge (coverage < 80% OR quality gate failed)"
  
- pipeline_stage: "Publish Artifacts"
  tasks:
    - "Build Docker image"
    - "Push to Azure Container Registry"
    - "Tag with git commit SHA"
  duration: "3-5 minutos"
  
- pipeline_stage: "Deploy to Staging"
  tasks:
    - "Deploy container to Azure App Service (Staging slot)"
    - "Run smoke tests"
    - "Run integration tests"
  duration: "10-15 minutos"
  failure_action: "Rollback deployment"
  
- pipeline_stage: "Deploy to Production"
  trigger: "Manual approval (Paula PO + João PM)"
  tasks:
    - "Blue/green deployment (zero-downtime)"
    - "Swap slots (Staging → Production)"
    - "Monitor for 30min (error rate, latency)"
  failure_action: "Auto-rollback if errors > 0.5%"
```

---

### Infrastructure as Code (IaC)
_Gerenciamento de infra_

**Exemplo**:
```yaml
- tool: "Terraform (Azure provider)"
  use_case: "Provisionar Azure resources (App Service, SQL Database, Redis)"
  benefits:
    - "Versionado (git) → auditável"
    - "Reproduzível (mesma infra em dev/staging/prod)"
    - "Declarativo (define estado desejado, não steps)"
  example:
    resource: "azurerm_app_service"
    config: |
      resource "azurerm_app_service" "app" {
        name = "my-app-${var.environment}"
        location = "East US"
        resource_group_name = azurerm_resource_group.rg.name
        app_service_plan_id = azurerm_app_service_plan.plan.id
      }
  
- practice: "Environment Parity (dev/staging/prod similares)"
  implementation: "Terraform workspaces (dev, staging, prod)"
  benefit: "Bugs de infra detectados early (não apenas em prod)"
  caveat: "Custo (3 ambientes), mas ROI positivo (menos outages)"
```

---

## 🧠 Learning & Skill Development

### Technologies to Learn (Roadmap)
_Skills prioritizadas_

**Exemplo**:
```yaml
- skill: "Kubernetes (container orchestration)"
  priority: "Alta"
  rationale: "Projetos grandes migrando para K8s (escalabilidade)"
  learning_path: "Kubernetes in Action (livro) + hands-on (AKS)"
  timeline: "Q1 2024"
  
- skill: "Domain-Driven Design (DDD)"
  priority: "Média-Alta"
  rationale: "Projetos complexos (bounded contexts, ubiquitous language)"
  learning_path: "DDD Eric Evans (livro) + workshop Wilson (Architect)"
  timeline: "Q2 2024"
  
- skill: "Rust (systems programming)"
  priority: "Baixa-Média"
  rationale: "Performance crítica (não mainstream ainda em Avanade)"
  learning_path: "The Rust Book + side project"
  timeline: "Q3-Q4 2024 (se tempo)"
```

---

## 🔗 Cross-References

### Artifacts Relacionados:
- Clean Code Task: `${AVANADE_TASK_CLEAN_CODE}`
- Debugging Guide: `${AVANADE_TASK_DEBUGGING_GUIDE}`
- Architecture Quality: `${AVANADE_TASK_ARCHITECTURE_QUALITY}`
- Story Template: `${AVANADE_STORY_TEMPLATE_YAML}`

### Agent Collaboration:
```yaml
supervisor: ${AVANADE_MEMORY_SUPERVISOR}
architect: ${AVANADE_MEMORY_ARCHITECT_WILSON}
qa: ${AVANADE_MEMORY_QA_CARLA}
sm: ${AVANADE_MEMORY_SM_ROBERTO}
po: ${AVANADE_MEMORY_PO_PAULA}
```

---

## 📌 Como Usar Esta Memória

### ✅ ANTES de implementar feature:
1. Consultar **Design Patterns** → escolher pattern adequado
2. Revisar **Tech Stack Preferences** → tecnologias validadas
3. Consultar **Code Quality Patterns** → SOLID, DRY, YAGNI

### ✅ DURANTE desenvolvimento:
1. Aplicar **Refactoring Techniques** → melhorar código continuamente
2. Considerar **Performance Optimization** → se aplicável
3. Seguir **Code Review Checklist** → auto-review antes de PR

### ✅ QUANDO debugar:
1. Usar **Debugging Techniques** → abordagem sistemática
2. Consultar **Common Error Patterns** → soluções conhecidas
3. Documentar novos bugs → atualizar memória

### ✅ APÓS code review:
1. **Atualizar memória** com novos patterns aprendidos
2. Documentar **Lessons Learned** → erros evitados
3. Compartilhar conhecimento → pair programming, tech talks

---

## 🏢 D365 CE Development Context - FTD Educação

### Stack D365 Ativo
```yaml
platform: "Dynamics 365 CE Online"
modules: [Sales (Spartan + PNLD), Customer Service (Hub SAC)]
publisher_prefix: "ftd"
plugin_namespace: "FTD.Plugins"
function_namespace: "FTD.Functions"
pcf_prefix: "FTD"
web_resource_prefix: "ftd_"
testing: "FakeXrmEasy + xUnit (plugins), Jest (PCF), Postman/Newman (APIs)"
```

### Ambiente Real FTD
- **9 solutions** segmentadas por tipo de componente (numeradas com ordem de dependência)
- **Repo**: FTD Dynamics (branch `dev` = source of truth, feature branches)
- **Pipeline**: Azure DevOps (Dev=manual plugins, OAT/Prod=pipeline, Prod requer GMUD via SMAX)
- **Plugins**: poucos, bug rate muito baixo após 1.5 ano de reestruturação
- **JS Web Resources**: em refatoração (assessment prévio), ambos base class (Late Binding) e legado
- **Power Automates**: críticos em revisão (especialmente fluxo de aprovação - muito extenso)
- **Azure Functions**: cálculos de proposta comercial, renovação automática
- **Batch updates**: projetos console para atualização em massa (executados em VM de infra)
- **Key Vault**: usado para Azure Functions (NÃO para Power Automate - usa environment variables)
- **Usuário de serviço**: ftdmaxflow (conexões e proprietário de flows)

### Simulador Comercial (Foco Principal)
- **Power Pages** (frontend) + CRM (motor)
- Cache client-side heavy (minimizar roundtrips Dataverse)
- Cálculos real-time em frontend (JavaScript/PowerFX)
- Validação backend: Azure Functions
- **Débounce**: ≤50 produtos → plugin síncrono; >50 → Azure Function
- MVP: adição individual de produtos (deadline 31/mar/2026)

### Integrações Ativas
- **TOTVS/Datasul**: ETLs bidirecionais (contas sync 1x/dia às 6h, código ERP)
- **ISA**: cadastro de produto (ISA ↔ TOTVS ↔ CRM - a redesenhar)
- **Adobe Sign**: plugin "Adobe Agreement" (em migração de tecnologia)
- **Tabela de log**: tabela genérica para qualquer integração
- **Tabela de fluxos gerenciais**: URLs de flows recursivos (matriz de serviços)
- Consumo apenas: time CRM recebe endpoints do time de integração (Thiago Veiga)
- Sistemas externos consultam CRM via OData/FetchXML (sem custom Actions expostas)

### D365 Artifacts
- Story Template: `d365-story-template.md`
- Dev Workflow (Plugin): `d365-plugin-dev.workflow.md`
- Dev Workflow (Integration): `d365-integration.workflow.md`
- Standards: `d365-development-standards.md`
- Code Review Checklist: `d365-code-review-checklist.md`
- **Knowledge Base**: `docs/ftd-knowledge-base.md` (LEITURA OBRIGATÓRIA)

### Critical Rules D365 FTD
- NUNCA async/await em plugins Dataverse
- NUNCA ColumnSet(true) - sempre especificar colunas
- SEMPRE depth check > 1
- SEMPRE ITracingService para logs
- SEMPRE FakeXrmEasy para testes (≥ 80% coverage)
- Secrets em Key Vault (nunca appsettings)
- Plugin execution < 2s (especialmente no pico nov-jan)
- Power Pages: minimizar consultas ao Dataverse (cache heavy)
- Nomear flows: "FTD - [Module] - [Action] - [Trigger]"

---