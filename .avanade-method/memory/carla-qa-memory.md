### Test Automation Patterns
_Estratégias de automação que funcionam_

**Exemplo**:
```yaml
- pattern: "Test Pyramid (70% unit, 20% integration, 10% E2E)"
  rationale:
    - "Unit tests: rápidos (ms), feedback imediato"
    - "Integration: validam contratos entre componentes"
    - "E2E: user flows críticos (happy paths)"
  implementation:
    unit: "Jest (JavaScript) ou xUnit (C#)"
    integration: "Testcontainers (DBs), WireMock (APIs)"
    e2e: "Playwright, Selenium, Cypress"
  metrics:
    - "Execution time: <5min (unit), <15min (integration), <30min (E2E)"
    - "Coverage: >80% unit, >60% integration"
  anti_pattern_avoided: "Inverted pyramid (muitos E2E → lento, flaky)"
  
- pattern: "Shift-Left Testing (QA desde Sprint 1)"
  activities:
    - "Carla participa de Planning (valida testabilidade de stories)"
    - "Acceptance criteria co-criados (Carla + Paula + Roberto)"
    - "Test automation em paralelo com dev (não após)"
  benefits:
    - "Bugs descobertos early (custo 10x menor que em produção)"
    - "Definition of Done inclui testes (não depois)"
  metrics: "Bugs em produção -60% (shift-left adotado)"
  
- pattern: "BDD (Behavior-Driven Development)"
  tool: "Cucumber, SpecFlow"
  example:
    feature: "User Login"
    scenario: |
      Given user is on login page
      When user enters valid credentials
      Then user is redirected to dashboard
  benefits:
    - "Testes legíveis por stakeholders (não apenas devs)"
    - "Living documentation (specs = tests)"
  cons: "Overhead de manutenção (Gherkin syntax)"
  best_for: "Projetos com stakeholders técnicos envolvidos"
```

---

### Test Coverage Strategies
_Como garantir cobertura adequada_

**Exemplo**:
```yaml
- metric: "Code Coverage (by lines)"
  target: ">80%"
  measurement: "SonarQube, Coveralls"
  interpretation:
    - ">90%: Excelente (mas watch for false security)"
    - "70-80%: Adequado (foco em críticos)"
    - "<70%: Insuficiente (riscos altos)"
  caveat: "Coverage NÃO garante qualidade (pode testar código mas não casos edge)"
  
- metric: "Mutation Testing (quality of tests)"
  tool: "Stryker.NET, PIT (Java)"
  concept: "Introduz bugs propositais → testes devem detectar"
  example:
    original_code: "if (age >= 18)"
    mutation: "if (age > 18)"
    expectation: "Testes devem FALHAR (detectar bug introduzido)"
  benefit: "Valida que testes são eficazes (não apenas executam código)"
  
- pattern: "Risk-Based Testing (priorizarabertura por risco)"
  high_priority:
    - "Payment processing (financial impact)"
    - "Authentication/Authorization (security)"
    - "Data migrations (irreversível se erro)"
  medium_priority:
    - "Reporting features (UX impact, não crítico)"
  low_priority:
    - "UI cosmetics (cor de botão, etc)"
  benefit: "ROI de testing otimizado (foco onde bugs custam mais)"
```

---

### Bug Triage & Management
_Processo de gestão de bugs_

**Exemplo**:
```yaml
- severity_classification:
  critical:
    definition: "Sistema inoperante, perda de dados, security breach"
    sla: "Fix em <4h (hotfix imediato)"
    example: "SQL injection vulnerability, payment processing down"
  high:
    definition: "Funcionalidade core quebrada, workaround difícil"
    sla: "Fix em <24h (próximo deploy)"
    example: "Login não funciona (50% dos usuários afetados)"
  medium:
    definition: "Funcionalidade secundária quebrada, workaround existe"
    sla: "Fix em próxima sprint"
    example: "Export to PDF falha (workaround: export to CSV)"
  low:
    definition: "Cosmético, edge case raro"
    sla: "Backlog (fix quando capacity)"
    example: "Tooltip truncado em resoluções < 1024px"
  
- priority_matrix: "Severity × Frequency"
  high_priority: "Critical severity OU High severity + High frequency"
  medium_priority: "Medium severity + Medium frequency"
  low_priority: "Low severity OU Rare occurrence"
  
- bug_lifecycle:
  1. "New: Bug reportado (Carla ou user)"
  2. "Triaged: Severity/priority atribuídos (Carla + Paula)"
  3. "Assigned: Developer atribuído (Tiago)"
  4. "In Progress: Dev trabalhando"
  5. "Fixed: Code committed, deployed to staging"
  6. "Verified: Carla valida fix (regression test)"
  7. "Closed: Deploy to production, monitoring"
```

---

### Quality Gates Configuration
_Gates obrigatórios antes de releases_

**Exemplo**:
```yaml
- gate: "Pre-Merge (Pull Request)"
  automated_checks:
    - "Unit tests passed (>80% coverage)"
    - "Code quality (SonarQube: 0 critical issues)"
    - "Security scan (OWASP: 0 high/critical vulns)"
  manual_checks:
    - "Code review approved (Tiago ou senior dev)"
  sla: "Blocker - merge bloqueado até passar"
  
- gate: "Pre-Staging Deployment"
  automated_checks:
    - "Integration tests passed"
    - "API contract tests passed (Pact)"
    - "Performance benchmarks (P95 < 200ms)"
  manual_checks:
    - "Smoke tests (Carla): critical paths funcionam"
  sla: "Blocker - deploy bloqueado até passar"
  
- gate: "Pre-Production Release"
  automated_checks:
    - "E2E tests passed (critical user flows)"
    - "Load testing (50k concurrent users, no errors)"
    - "Accessibility scan (WCAG AA compliance - Sofia valida)"
  manual_checks:
    - "UAT sign-off (Paula + stakeholders)"
    - "Release notes reviewed (communications team)"
    - "Rollback plan documented"
  sla: "Blocker - deploy bloqueado até passar"
  approval: "Paula (PO) + João (PM) aprovam formalmente"
  
- gate: "Post-Production Monitoring (24h Hypercare)"
  checks:
    - "Error rate < 0.1% (Application Insights)"
    - "P95 latency < 300ms"
    - "No critical bugs reported (support tickets)"
  action_if_fail: "Rollback to previous version (automated)"
```

---

## 🐛 Bug Patterns & Root Causes

### Recurring Bug Patterns
_Bugs que aparecem frequentemente e root causes_

**Exemplo**:
```yaml
- bug_pattern: "Null Reference Exceptions"
  frequency: "Alta (30% dos bugs)"
  root_cause: "Validação de input insuficiente, null checks faltando"
  prevention:
    - "Static analysis (null safety checks - C# nullable reference types)"
    - "Defensive programming (guard clauses)"
    - "Code review checklist item: 'Null checks in place?'"
  impact: "Redução 30% → 10% após mitigations"
  
- bug_pattern: "Race Conditions (concurrency bugs)"
  frequency: "Média (15% dos bugs em sistemas distribuídos)"
  root_cause: "Shared state, locks inadequados, async/await misuse"
  prevention:
    - "Immutable data structures (evitar shared mutable state)"
    - "Proper locking (ReaderWriterLock, semaphores)"
    - "Load testing early (expõe race conditions)"
  detection: "Difícil (flaky tests, intermittent failures)"
  impact: "Redução via code reviews focadas + load testing"
  
- bug_pattern: "SQL Injection (security)"
  frequency: "Baixa (5% mas CRITICAL severity)"
  root_cause: "Concatenação de strings em queries (não parameterizado)"
  prevention:
    - "ORMs (Entity Framework, Hibernate) → parameterização automática"
    - "Code review checklist: 'SQL queries parameterizadas?'"
    - "SAST tools (Static Application Security Testing)"
  impact: "Zero SQL injections após SAST enforcement"
  
- bug_pattern: "UI Rendering Issues (mobile/browser inconsistencies)"
  frequency: "Média (20% dos bugs UX)"
  root_cause: "Browser compatibility, responsive design edge cases"
  prevention:
    - "Cross-browser testing (Playwright em Chrome/Firefox/Safari)"
    - "Responsive testing (Browserstack: múltiplos devices)"
    - "Design system (Fluent UI) → consistent components"
  impact: "Redução 20% → 8% após design system adoption"
```

---

### Flaky Tests Management
_Como lidar com testes instáveis_

**Exemplo**:
```yaml
- cause: "Timing issues (async operations)"
  symptom: "Test passa 80% das vezes, falha 20% (intermittent)"
  fix:
    - "Explicit waits (waitForElement) em vez de sleep(5000)"
    - "Retry logic (Playwright: expect().toBeVisible({timeout: 10000}))"
  prevention: "Code review: evitar sleeps, usar waits explícitos"
  
- cause: "Test order dependency (tests não isolados)"
  symptom: "Test A passa sozinho, falha se executado após Test B"
  fix:
    - "Setup/teardown adequados (limpar DB, reset state)"
    - "Testes independentes (cada test cria próprios dados)"
  prevention: "Run tests em ordem aleatória (detecta dependencies)"
  
- cause: "External dependencies (APIs de terceiros)"
  symptom: "Test falha quando API externa está down"
  fix:
    - "Mocking/stubbing (WireMock, Mockito)"
    - "Contract testing (Pact) → valida contratos, não implementação"
  prevention: "Testes não devem depender de serviços externos (exceto E2E em staging)"
  
- policy: "Flaky Test Budget"
  rule: "Max 2% flaky tests tolerados (98% success rate em CI)"
  action_if_exceeded: "Quarantine flaky tests (disable) até fixados"
  owner: "Carla identifica, Tiago fixa"
```

---

## 📊 Test Metrics & KPIs

### Testing Effectiveness Metrics
_Métricas que Carla monitora_

**Exemplo**:
```yaml
- metric: "Defect Detection Rate (DDR)"
  formula: "Bugs found in QA / Total bugs (QA + Production)"
  target: ">90%"
  current: "87%"
  interpretation: "87% dos bugs encontrados antes de produção (bom, mas pode melhorar)"
  action: "Increase exploratory testing, improve test coverage"
  
- metric: "Defect Removal Efficiency (DRE)"
  formula: "Bugs fixed / Bugs reported"
  target: ">95%"
  current: "92%"
  interpretation: "8% dos bugs reportados não são fixados (backlog creep)"
  action: "Bug triage rigoroso (fechar won't-fix explicitamente)"
  
- metric: "Mean Time to Detect (MTTD)"
  measurement: "Tempo médio entre bug introduzido (commit) e detectado"
  target: "<24h"
  current: "36h"
  interpretation: "Bugs demoram 1.5 dias para serem detectados"
  action: "CI/CD mais frequente, automated tests em cada PR"
  
- metric: "Mean Time to Resolve (MTTR)"
  measurement: "Tempo médio entre bug detectado e fixado"
  target: "<48h (para high/critical)"
  current: "40h"
  interpretation: "Dentro do target (good)"
  
- metric: "Test Execution Time"
  measurement: "Tempo total de test suite (CI/CD pipeline)"
  target: "<15min (para feedback rápido)"
  current: "22min"
  interpretation: "Pipeline lento → devs não rodam testes localmente"
  action: "Otimizar testes lentos, paralelização"
```

---

## 🛠️ Testing Tools & Technologies

### Tool Stack Preferences
_Ferramentas validadas_

**Exemplo**:
```yaml
- category: "Unit Testing"
  tool: "Jest (JavaScript), xUnit (C#), JUnit (Java)"
  rationale: "Padrão da indústria, bem suportado"
  
- category: "E2E Testing"
  tool: "Playwright (preferido), Cypress (alternativo)"
  rationale:
    - "Playwright: multi-browser, auto-wait, trace viewer"
    - "Cypress: developer-friendly, time-travel debugging"
  decision: "Playwright para novos projetos (melhor tooling)"
  
- category: "API Testing"
  tool: "Postman (manual), RestAssured (automation), Pact (contract)"
  rationale: "Postman para exploratory, RestAssured para CI/CD, Pact para microservices"
  
- category: "Performance Testing"
  tool: "k6, JMeter, Azure Load Testing"
  rationale: "k6 para scripting (JS), JMeter para GUI, Azure Load Testing para cloud-scale"
  
- category: "Security Testing"
  tool: "OWASP ZAP, SonarQube, Snyk"
  rationale: "ZAP para DAST, SonarQube para SAST, Snyk para dependencies"
  
- category: "Accessibility Testing"
  tool: "axe DevTools, Pa11y, Lighthouse"
  rationale: "axe para CI/CD, Pa11y para automation, Lighthouse para audits"
  collaboration: "Sofia (UX) valida WCAG compliance"
```

---

## 🔄 Continuous Testing in CI/CD

### CI/CD Pipeline Integration
_Como testes integram no pipeline_

**Exemplo**:
```yaml
- stage: "Build"
  tests: "Unit tests (Jest, xUnit)"
  duration: "3min"
  failure_action: "Block PR merge (red status)"
  
- stage: "Integration"
  tests: "Integration tests (Testcontainers)"
  duration: "8min"
  failure_action: "Block deployment to staging"
  
- stage: "Staging Deployment"
  tests: "Smoke tests (critical paths)"
  duration: "5min"
  failure_action: "Auto-rollback staging deployment"
  
- stage: "E2E Testing (Staging)"
  tests: "Playwright E2E suite"
  duration: "15min"
  failure_action: "Block promotion to production"
  
- stage: "Production Deployment"
  tests: "Canary deployment (5% traffic) + monitoring"
  duration: "30min observation"
  failure_action: "Auto-rollback if error rate > 0.5%"
  
- stage: "Post-Deployment"
  tests: "Smoke tests (production), synthetic monitoring"
  duration: "Continuous"
  failure_action: "Alert on-call (PagerDuty)"
```

---

## 🎯 Exploratory Testing Insights

### Exploratory Testing Charters
_Sessões de teste exploratório estruturadas_

**Exemplo**:
```yaml
- charter: "Explore payment flow for edge cases"
  duration: "60 minutos (timeboxed)"
  focus_areas:
    - "Múltiplos cartões (adicionar, remover, trocar default)"
    - "Pagamentos falhados (cartão recusado, timeout)"
    - "Concurrency (2 pagamentos simultâneos)"
  bugs_found: 3
  findings:
    - "Bug: Timeout de pagamento não mostra mensagem clara ao usuário"
    - "UX issue: Remover cartão não pede confirmação (risky)"
    - "Bug: Concurrent payments causam race condition"
  
- charter: "Explore mobile responsiveness (iOS Safari)"
  duration: "45 minutos"
  focus_areas:
    - "Formulários (keyboard overlap, input focus)"
    - "Navigation (gestures, back button)"
    - "Performance (scroll lag, animations)"
  bugs_found: 2
  findings:
    - "Bug: Keyboard sobrepõe botão Submit (não scrollable)"
    - "Performance: Scroll lag em listas longas (>100 items)"
```

---

## 🔗 Cross-References

### Artifacts Relacionados:
- Clean Code Task: `${AVANADE_TASK_CLEAN_CODE}`
- Architecture Quality: `${AVANADE_TASK_ARCHITECTURE_QUALITY}`
- Debugging Guide: `${AVANADE_TASK_DEBUGGING_GUIDE}`
- UX Checklist: `${AVANADE_TASK_UX_CHECKLIST}`

### Agent Collaboration:
```yaml
supervisor: ${AVANADE_MEMORY_SUPERVISOR}
dev: ${AVANADE_MEMORY_DEV_TIAGO}
architect: ${AVANADE_MEMORY_ARCHITECT_WILSON}
ux: ${AVANADE_MEMORY_UX_SOFIA}
po: ${AVANADE_MEMORY_PO_PAULA}
```

---

## 📌 Como Usar Esta Memória

### ✅ ANTES de Sprint Planning:
1. Consultar **Test Automation Patterns** → estratégia de testes para sprint
2. Revisar **Quality Gates** → garantir compliance
3. Consultar **Bug Patterns** → prevenção proativa

### ✅ DURANTE Sprint:
1. Aplicar **Test Coverage Strategies** → adequada cobertura
2. Usar **Flaky Tests Management** → estabilizar testes
3. Executar **Exploratory Testing Charters** → descobrir edge cases

### ✅ ANTES de Release:
1. Validar **Quality Gates** → todos passaram
2. Revisar **Test Metrics** → quality indicators saudáveis
3. Executar **Exploratory Testing** → validação final

### ✅ APÓS Bugs em Produção:
1. **Root Cause Analysis** → documentar em Bug Patterns
2. **Atualizar memória** → prevention strategies
3. **Refinar Quality Gates** → prevenir recorrência

---

## 🏢 D365 CE QA Context - FTD Educação

### Estado Atual de Qualidade FTD
- Bug rate **muito baixo** após 1.5 ano de reestruturação (2 bugs/mês recente)
- Refatoração em andamento: Power Automate, JS web resources, plugins
- Assessment de código legado parcialmente concluído
- **Dataverse em nível crítico de armazenamento**

### Áreas de Risco - Foco de QA
1. **Simulador Power Pages**: cálculos frontend vs backend (devem bater)
2. **Débounce**: plugin sync (≤50 produtos) vs Azure Function (>50) - testar boundary
3. **Pico sazonal (nov-jan)**: ~5.000 contratos/dia - performance under load
4. **50+ templates Word**: validação de cada tipo de contrato/aditivo
5. **Fluxo de aprovação**: 4 níveis de alçada com regras complexas
6. **Integração TOTVS**: sync de contas/CNPJ - dados não devem divergir
7. **Tabela de produtos**: filtros devem excluir produtos de outras escolas
8. **Multi-canal**: mesma proposta vendida por vários canais

### Processo de Bug Atual FTD
- Story bugs → DevOps (vinculado à user story)
- Produção bugs → N1 (sustentação portal externo) → triagem → se necessário, bug no DevOps

### Cenários de Teste Críticos FTD
- Proposta com 200 produtos (stress test de performance)
- Revisão de proposta até 27 vezes (integridade de dados)
- Copiar proposta anterior com código substituto + reajuste IPCA
- Aprovação passando por todos 4 níveis
- Produto personalizado visível apenas para escola específica
- Validação CNPJ duplicado na criação de conta
- Matriz de serviços (15 min de processamento - timeout handling)

### D365 Quality Gates
- Solution Checker: 0 critical, 0 high
- Plugin unit tests: FakeXrmEasy ≥ 80% coverage
- Power Pages: cálculos frontend = cálculos backend
- Plugin execution < 2s (mesmo no pico sazonal)
- **Knowledge Base**: `docs/ftd-knowledge-base.md` (LEITURA OBRIGATÓRIA)

---