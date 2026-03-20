# ============================================================
# D365 Code Review Checklist - Avanade Method v4.29.0
# ============================================================
# Owner: Carla QA + Tiago Dev
# When: Code review de qualquer customização pro-code D365

---

# 🔍 D365 Code Review Checklist

## Plugin C# Review

### Architecture
- [ ] Plugin registrado no stage correto (Pre-Validation / Pre-Op / Post-Op)
- [ ] Sync vs Async escolhido corretamente para o caso de uso
- [ ] Filtering attributes limitam execução desnecessária
- [ ] Single Responsibility - plugin faz UMA coisa

### Code Quality
- [ ] Herda de PluginBase / IPlugin implementado corretamente
- [ ] `ITracingService` usado para todo logging
- [ ] `InvalidPluginExecutionException` para erros de negócio
- [ ] Depth check: `if (context.Depth > 1) return;`
- [ ] Target entity validado antes de uso
- [ ] Null checks adequados em ogni attribute access
- [ ] Early-bound entities (não magic strings)
- [ ] Constantes para GUIDs e option set values
- [ ] Sem `try {} catch (Exception) {}` genérico que engole erros

### Security
- [ ] Sem credenciais hardcoded
- [ ] Sem SQL injection (se usar FetchXML dinâmico)
- [ ] Input validation em Custom APIs
- [ ] Sem exposição de dados sensíveis em trace logs
- [ ] Sem uso de unsecure configuration para secrets

### Performance
- [ ] Sem query dentro de loop (N+1)
- [ ] `ColumnSet` especificado (nunca `new ColumnSet(true)`)
- [ ] `ExecuteMultipleRequest` para batch operations
- [ ] Retrieve com top/count limitados
- [ ] Pre/Post images com apenas atributos necessários

### Testing
- [ ] FakeXrmEasy unit tests presentes
- [ ] Happy path testado
- [ ] Error paths testados
- [ ] Edge cases testados (null, empty, boundary)
- [ ] Cobertura ≥ 80%

---

## PCF Control Review

### Architecture
- [ ] Manifest.xml propriedades tipadas corretamente
- [ ] `init/updateView/getOutputs/destroy` implementados
- [ ] React components (se usado) seguem patterns

### Code Quality
- [ ] TypeScript strict mode
- [ ] Sem `any` types desnecessários
- [ ] Event handlers com cleanup em `destroy`
- [ ] State management claro (React state ou ComponentFramework)

### UX
- [ ] Responsive design (Web + Tablet + Phone)
- [ ] Keyboard navigation funcional
- [ ] Aria labels para acessibilidade
- [ ] Loading states adequados
- [ ] Error states com mensagens claras

### Performance
- [ ] Bundle size < 5MB
- [ ] Sem re-renders desnecessários
- [ ] Resources carregados lazy quando possível
- [ ] Sem memory leaks (cleanup em destroy)

### Testing
- [ ] Jest unit tests presentes
- [ ] Component rendering testado
- [ ] User interactions testadas
- [ ] Cobertura ≥ 70%

---

## Azure Function Review

### Architecture
- [ ] Trigger type correto para o caso de uso
- [ ] DI configurado corretamente (Program.cs)
- [ ] Services abstraídos com interfaces

### Security
- [ ] Authentication via Managed Identity ou App Registration
- [ ] Secrets em Key Vault (nunca appsettings.json)
- [ ] Authorization level configurada (Function/Anonymous/Admin)
- [ ] Input validation em HTTP triggers
- [ ] CORS configurado se necessário

### Resilience
- [ ] Retry policy implementada (Polly)
- [ ] Circuit breaker para APIs externas
- [ ] Timeout configurado
- [ ] Dead-letter handling (se Service Bus)
- [ ] Idempotency garantida

### Performance
- [ ] Dataverse API throttling respeitado (6000 req/5min)
- [ ] Connection pooling para HttpClient
- [ ] Batch processing quando possível
- [ ] Cold start minimizado (Premium plan ou warmup)

### Observability
- [ ] Application Insights configurado
- [ ] Structured logging (ILogger)
- [ ] Correlation ID propagado
- [ ] Custom metrics para business KPIs
- [ ] Health check endpoint

### Testing
- [ ] xUnit + Moq unit tests
- [ ] Integration tests com Dataverse (test env)
- [ ] Cobertura ≥ 80%

---

## JavaScript Web Resource Review

### Code Quality
- [ ] Namespace pattern: `FTD.[Module].[Function]`
- [ ] Sem variáveis globais (IIFE ou module pattern)
- [ ] Xrm.WebApi para CRUD operations
- [ ] Promise handling (async/await ou .then/.catch)
- [ ] Sem alerts - usar form notifications

### Performance
- [ ] Sem sync XMLHttpRequest (deprecated)
- [ ] Minimal form OnLoad scripts
- [ ] Lazy loading para operações pesadas
- [ ] Events deregistered quando não necessários

### D365 API Usage
- [ ] `formContext` usado (não `Xrm.Page` deprecated)
- [ ] `executionContext.getFormContext()` em event handlers
- [ ] `getAttribute().getValue()` com null check
- [ ] `getControl().setVisible()` em vez de DOM manipulation

---

## Power Automate Flow Review

### Design
- [ ] Nome: "FTD - [Module] - [Action] - [Trigger]"
- [ ] Scope Try-Catch-Finally
- [ ] Variables inicializadas no topo
- [ ] Connection references (não connections diretas)

### Error Handling
- [ ] Run-after configurado para failure/timeout/skipped
- [ ] Error notification (email/Teams/log)
- [ ] Retry policy em HTTP actions
- [ ] Timeout em actions longas

### Performance
- [ ] Concurrency control adequado
- [ ] Pagination handling para large datasets
- [ ] Apply-to-each minimizado (prefer batch)
- [ ] Conditions otimizadas (early exit)

---

## Review Verdict

| Dimension | Score (1-5) | Notes |
|-----------|-------------|-------|
| Architecture | | |
| Code Quality | | |
| Security | | |
| Performance | | |
| Testing | | |
| Documentation | | |

**Overall**: ⬜ APPROVED / ⬜ APPROVED WITH COMMENTS / ⬜ REQUEST CHANGES / ⬜ REJECTED

**Reviewer**: 
**Date**: YYYY-MM-DD
