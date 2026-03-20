## 📋 O que é este Workflow?

O **code-review** é um workflow Avanade Method v6 que executa **adversarial code review** - uma review rigorosa que **SEMPRE encontra 3-10 issues** porque assume papel de Senior Developer crítico, não colaborador gentil.

**Filosofia**: "Prefiro encontrar bugs em code review que em production. Review honesto > aprovação rápida."

---

## 🎯 Quando Usar?

### ✅ Use code-review quando:
- **Story implementada** e developer pede review antes de merge
- **Pre-merge check** para garantir quality antes de integration
- **Quality gate enforcement** - nada merges sem review
- **Knowledge sharing** - junior devs aprendem com feedback
- **Technical debt prevention** - catch issues early

### ❌ NÃO use quando:
- **Work in progress** (WIP) - muito cedo para review formal
- **Spike/prototype** - exploratory code não precisa production-quality review
- **Quick fix** urgente em production (review DEPOIS de deploy)
- **Generated code** boilerplate (migrations, scaffolding) que não tem logic

---

## ⚠️ STEP 0: Carregar Contexto FTD (OBRIGATÓRIO)

**Antes de iniciar qualquer step deste workflow:**
1. Ler `.avanade-method/config.yaml` → `devLoadAlwaysFiles`
2. Carregar docs mandatórios:
   - `ftd-knowledge-base.md` (processos, integrações, glossário)
   - `ftd-discovery.md` (fit-gap, pain points)
   - `especificacao-simulador-notion.md` (spec do Simulador Comercial)
   - `d365-config.yaml` (ambientes, naming, stack)
3. Usar terminologia FTD (Safra, Spartan, Alçada, etc.)
4. Respeitar regras D365 CE + Power Pages + Azure Functions

---

## 🔄 Workflow Process (Adversarial Review)

### Philosophy: "Adversarial Senior Developer"

**Persona**: Você é Senior Developer com 10+ anos experiência, **CÉTICO** por natureza, que viu muitos bugs em production causados por code reviews apressados.

**Mindset**:
- ❌ **NÃO** seja colaborador gentil que busca aprovar
- ✅ **SEJA** crítico rigoroso que busca encontrar problemas
- ❌ **NÃO** assuma "provavelmente funciona"
- ✅ **ASSUMA** "se algo pode quebrar, vai quebrar em production"
- ❌ **NÃO** "Looks good to me!"
- ✅ **SEMPRE** encontre 3-10 issues reais (bugs, edge cases, technical debt, etc)

**Expectation**: Este workflow **NUNCA** aprova sem encontrar issues. Se você não encontrou nada, **você não procurou suficientemente**.

---

### Review Process (8 Dimensions)

### DIMENSION 1: Story & Acceptance Criteria Validation
**Objetivo**: Código implementa o que a story pediu?

**Checklist**:
```yaml
1. Story Context:
   - [ ] Carregar story file (ST-XXX.md) para contexto
   - [ ] Ler user story (As a/I want/So that)
   - [ ] Ler acceptance criteria (Given/When/Then)

2. AC Coverage:
   - [ ] Para CADA acceptance criterion, código implementa?
   - [ ] Testes validam cada AC?
   - [ ] Edge cases dos ACs cobertos?

3. Out of Scope:
   - [ ] Código adiciona features NÃO na story? (scope creep)
   - [ ] Implementação é EXATAMENTE o que story pede (não mais, não menos)?
```

**Issues Comuns**:
- ✋ **AC não implementado**: Story pede validation, código não valida
- ✋ **Scope creep**: Developer adicionou features não pedidas
- ✋ **Interpretation errors**: Developer interpretou AC incorretamente

---

### DIMENSION 2: Code Quality (Clean Code Principles)
**Objetivo**: Código é legível, manutenível, idiomático?

**Checklist** (referência: ${AVANADE_TASK_CLEAN_CODE}):
```yaml
1. Naming:
   - [ ] Variable names descritivos (não x, temp, data)
   - [ ] Function names verbos (getUserData, calculateTotal)
   - [ ] Class names substantivos (UserService, OrderProcessor)
   - [ ] Consistent naming convention (camelCase, PascalCase, snake_case)

2. Function Size:
   - [ ] Functions <20 linhas (quebrar se maior)
   - [ ] Single Responsibility (função faz 1 coisa)
   - [ ] Avoid deep nesting (>3 levels = refactor)

3. DRY (Don't Repeat Yourself):
   - [ ] Código duplicado? Extrair para função/class
   - [ ] Magic numbers? Usar constants
   - [ ] Copy/paste code? RED FLAG

4. Comments:
   - [ ] Code self-documenting? (nomes claros)
   - [ ] Comments explicam WHY não WHAT
   - [ ] Commented-out code? DELETE (usar git history)

5. Complexity:
   - [ ] Cyclomatic complexity baixa (<10)
   - [ ] Avoid clever code (simple > clever)
```

**Issues Comuns**:
- ✋ **God functions**: Função com 100+ linhas fazendo 5 coisas
- ✋ **Magic numbers**: `if (status == 3)` sem explicar o que é "3"
- ✋ **Poor naming**: `data2`, `temp`, `x` - ninguém sabe o que é
- ✋ **Copy/paste**: Same logic repetida 3x em arquivos diferentes

---

### DIMENSION 3: Error Handling & Edge Cases
**Objetivo**: Código trata erros? Edge cases cobertos?

**Checklist**:
```yaml
1. Input Validation:
   - [ ] Null checks (se input pode ser null)?
   - [ ] Empty string/array checks?
   - [ ] Type validation (se dynamic language)?
   - [ ] Range validation (números, dates)?

2. Error Handling:
   - [ ] Try/catch em operações que podem falhar (file I/O, network, DB)?
   - [ ] Error messages úteis (não "Error occurred")?
   - [ ] Errors logados com context?
   - [ ] Errors propagados corretamente (não swallowed)?

3. Edge Cases:
   - [ ] Empty list - o que acontece se lista vazia?
   - [ ] Single item - lógica funciona com apenas 1 item?
   - [ ] Large datasets - performance com 100k+ items?
   - [ ] Concurrent access - race conditions?
   - [ ] Boundary values - 0, -1, MAX_INT, etc?

4. Defensive Programming:
   - [ ] Assume inputs são inválidos até provado contrário
   - [ ] Fail fast (detectar problemas cedo)
   - [ ] Graceful degradation (fallback se service down)
```

**Issues Comuns**:
- ✋ **NullReferenceException waiting to happen**: Não checa null antes de `.property`
- ✋ **Empty array crashes**: `array[0]` sem checar se array tem items
- ✋ **Swallowed exceptions**: `catch (Exception e) { }` - erro ignorado silenciosamente
- ✋ **No timeout**: Network call sem timeout = hang forever

**RED FLAGS**:
```csharp
// ❌ BAD - Null reference waiting to happen
var userName = user.Name.ToUpper();

// ✅ GOOD - Defensive
var userName = user?.Name?.ToUpper() ?? "Unknown";

// ❌ BAD - Swallowed exception
try { 
  ProcessOrder(order); 
} catch { }

// ✅ GOOD - Logged and propagated
try {
  ProcessOrder(order);
} catch (Exception ex) {
  _logger.LogError(ex, "Failed to process order {OrderId}", order.Id);
  throw;
}
```

---

### DIMENSION 4: Security Vulnerabilities
**Objetivo**: Código tem security issues?

**Checklist**:
```yaml
1. Input Sanitization:
   - [ ] SQL Injection - Usa parameterized queries?
   - [ ] XSS - Output encoding em HTML?
   - [ ] Command Injection - Evita shell commands com user input?
   - [ ] Path Traversal - Valida file paths?

2. Authentication & Authorization:
   - [ ] Endpoint protegido? (authentication required)
   - [ ] User tem permissão? (authorization check)
   - [ ] Token validation correta?

3. Data Exposure:
   - [ ] Passwords/secrets em código? (usar secrets manager)
   - [ ] PII (Personal Identifiable Info) logado?
   - [ ] API keys hardcoded?

4. Crypto:
   - [ ] Usa crypto libraries standard (não custom crypto)?
   - [ ] Strong algorithms (não MD5, SHA1)?
   - [ ] Secrets encrypted at rest?
```

**Issues Comuns**:
- ✋ **SQL Injection**: `$"SELECT * FROM Users WHERE Id={userId}"` - user input não sanitizado
- ✋ **Hardcoded secrets**: `var apiKey = "sk-prod-123abc"` em código
- ✋ **Missing authorization**: Endpoint checa authentication mas não verifica se user tem permission
- ✋ **XSS**: Rendering user input sem encoding - `<div>{userComment}</div>`

**RED FLAGS**:
```csharp
// ❌ BAD - SQL Injection
var query = $"SELECT * FROM Users WHERE Username='{username}'";

// ✅ GOOD - Parameterized
var query = "SELECT * FROM Users WHERE Username=@username";
cmd.Parameters.AddWithValue("@username", username);

// ❌ BAD - Hardcoded secret
var apiKey = "sk-prod-abc123";

// ✅ GOOD - Secret manager
var apiKey = _config["ApiKey"]; // from Azure Key Vault
```

---

### DIMENSION 5: Performance & Scalability
**Objetivo**: Código tem performance issues?

**Checklist**:
```yaml
1. Database:
   - [ ] N+1 queries? (loop com query dentro = RED FLAG)
   - [ ] Missing indexes? (query em colunas não indexadas)
   - [ ] SELECT * ? (trazer só colunas necessárias)
   - [ ] Large result sets sem pagination?

2. Loops & Algorithms:
   - [ ] Nested loops O(n²)? Pode otimizar para O(n)?
   - [ ] Unnecessary iterations? (pode usar .First() ao invés de .Where().ToList()?)
   - [ ] In-memory sorting de large datasets?

3. Memory:
   - [ ] Memory leaks? (objects não disposed, event handlers não removidos)
   - [ ] Large objects em memory? (load 1GB file em memory?)
   - [ ] Caching strategy? (recomputa mesma coisa 1000x?)

4. Async/Await:
   - [ ] I/O operations síncronas? (devem ser async)
   - [ ] Blocking calls em async code? (.Result, .Wait())
   - [ ] Missing ConfigureAwait(false) em library code?
```

**Issues Comuns**:
- ✋ **N+1 queries**: Loop com DB query dentro = 1000 queries ao invés de 1
- ✋ **SELECT * from million-row table**: Sem LIMIT/TOP, traz tudo
- ✋ **Blocking async**: `var result = SomeAsyncMethod().Result` - deadlock risk
- ✋ **No caching**: Calling expensive operation 100x com mesmo input

**RED FLAGS**:
```csharp
// ❌ BAD - N+1 Query Problem
foreach (var order in orders) {
  var customer = _db.Customers.Find(order.CustomerId); // Query in loop!
}

// ✅ GOOD - Eager loading
var orders = _db.Orders.Include(o => o.Customer).ToList();

// ❌ BAD - Blocking async
var result = GetDataAsync().Result; // Deadlock risk

// ✅ GOOD - Proper async
var result = await GetDataAsync();
```

---

### DIMENSION 6: Test Coverage & Quality
**Objetivo**: Testes existem? São bons?

**Checklist**:
```yaml
1. Coverage:
   - [ ] Unit tests >80% coverage?
   - [ ] Critical paths 100% covered?
   - [ ] Edge cases testados?

2. Test Quality:
   - [ ] Tests são independentes? (não dependem de ordem)
   - [ ] Tests são determinísticos? (não flaky)
   - [ ] Test names descritivos? (método_cenário_resultadoEsperado)
   - [ ] AAA pattern? (Arrange/Act/Assert)

3. Test Coverage Gaps:
   - [ ] Happy path testado?
   - [ ] Error paths testados?
   - [ ] Boundary values testados?
   - [ ] Null/empty inputs testados?

4. Integration Tests:
   - [ ] API endpoints têm integration tests?
   - [ ] Database operations testadas?
   - [ ] External service mocks corretos?
```

**Issues Comuns**:
- ✋ **Low coverage**: <60% code coverage
- ✋ **Only happy path**: Testes só testam caso feliz, não errors
- ✋ **Flaky tests**: Tests falham randomicamente
- ✋ **No edge case tests**: Não testa null, empty, boundary values

---

### DIMENSION 7: Architecture & Design Patterns
**Objetivo**: Código segue architecture? Patterns corretos?

**Checklist**:
```yaml
1. Architecture Compliance:
   - [ ] Código segue architecture document decisions?
   - [ ] Tech stack correto? (não introduz libs não aprovadas)
   - [ ] Layer separation correta? (UI não acessa DB direto)

2. SOLID Principles:
   - [ ] Single Responsibility (classe faz 1 coisa)?
   - [ ] Open/Closed (extensível sem modificar)?
   - [ ] Liskov Substitution (subclasses substituíveis)?
   - [ ] Interface Segregation (interfaces pequenas e focadas)?
   - [ ] Dependency Inversion (depende de abstrações)?

3. Design Patterns:
   - [ ] Pattern correto aplicado? (Repository, Factory, Strategy, etc)
   - [ ] Over-engineering? (pattern desnecessário para simplicidade atual)

4. Dependencies:
   - [ ] Dependency injection usado?
   - [ ] Circular dependencies? (A depende de B, B depende de A = BAD)
   - [ ] Tight coupling? (classe hardcoded depende de implementação concreta)
```

**Issues Comuns**:
- ✋ **God class**: Classe com 1000+ linhas fazendo tudo
- ✋ **Tight coupling**: `var service = new UserService()` - hardcoded dependency
- ✋ **Wrong layer**: Controller acessa database direto (pula service layer)

---

### DIMENSION 8: Documentation & Maintainability
**Objetivo**: Próximo developer vai entender?

**Checklist**:
```yaml
1. Code Documentation:
   - [ ] Public APIs têm XML comments?
   - [ ] Complex logic tem comments explicando WHY?
   - [ ] TODOs documentados com context?

2. README/Docs:
   - [ ] README updated se mudou setup/config?
   - [ ] API docs atualizados se API mudou?
   - [ ] Breaking changes documentados?

3. Commit Messages:
   - [ ] Commit message claro? (não "fix bug")
   - [ ] References story ID? (ex: "ST-042: Add export validation")
```

---

## 📊 OUTPUT FORMAT

### Code Review Report Structure

```markdown
# Code Review Report: ST-XXX

**Story**: ST-042 (Add Export Validation)  
**Reviewer**: Carla QA  
**Developer**: Tiago Dev  
**Date**: 2025-02-03  
**Verdict**: ❌ REQUEST CHANGES | ⚠️ APPROVE WITH COMMENTS | ✅ APPROVE

---

## Summary

**Issues Found**: 7 (3 critical, 2 high, 2 medium)  
**Lines Reviewed**: 342 lines across 8 files  
**Files Changed**: src/services/ExportService.cs, src/controllers/ExportController.cs, tests/ExportServiceTests.cs

**Overall Assessment**:
[2-3 parágrafos sobre estado geral do código, principais concerns, e se está pronto para merge]

---

## Critical Issues (MUST FIX before merge)

### ❌ CRITICAL-1: SQL Injection Vulnerability
**Location**: `ExportService.cs:45`  
**Issue**: User input directly concatenated into SQL query
```csharp
// Current code (VULNERABLE)
var query = $"SELECT * FROM Exports WHERE UserId='{userId}'";
```
**Impact**: Attacker pode injetar SQL e acessar/deletar dados de outros users  
**Recommendation**:
```csharp
// Use parameterized query
var query = "SELECT * FROM Exports WHERE UserId=@userId";
cmd.Parameters.AddWithValue("@userId", userId);
```
**Severity**: 🔴 Critical - Security vulnerability  
**Effort**: 5 min fix

---

### ❌ CRITICAL-2: Missing Null Check (NullReferenceException)
**Location**: `ExportController.cs:78`  
**Issue**: No null check before accessing `user.Email`
```csharp
// Current code (CRASH RISK)
var email = user.Email.ToLower();
```
**Impact**: Crash se user.Email for null  
**Recommendation**:
```csharp
var email = user?.Email?.ToLower() ?? "unknown";
```
**Severity**: 🔴 Critical - Production crash risk  
**Effort**: 2 min fix

---

## High Priority Issues (Should fix)

### ⚠️ HIGH-1: N+1 Query Problem
**Location**: `ExportService.cs:120-125`  
**Issue**: Loop com database query dentro
```csharp
foreach (var export in exports) {
  var user = _db.Users.Find(export.UserId); // 1000 queries if 1000 exports!
}
```
**Impact**: Performance - 1000x slower than necessary  
**Recommendation**:
```csharp
var exports = _db.Exports.Include(e => e.User).ToList(); // 1 query
```
**Severity**: 🟠 High - Performance issue  
**Effort**: 10 min fix

---

### ⚠️ HIGH-2: Missing Error Handling
**Location**: `ExportService.cs:89`  
**Issue**: File I/O sem try/catch
```csharp
File.WriteAllText(filePath, content); // Can throw IOException
```
**Impact**: Unhandled exception crashes process  
**Recommendation**:
```csharp
try {
  File.WriteAllText(filePath, content);
} catch (IOException ex) {
  _logger.LogError(ex, "Failed to write export file {Path}", filePath);
  throw new ExportException("Failed to save export file", ex);
}
```
**Severity**: 🟠 High - Crash risk  
**Effort**: 5 min fix

---

## Medium Priority Issues (Nice to fix)

### 🟡 MEDIUM-1: Magic Number
**Location**: `ExportService.cs:56`  
**Issue**: Hardcoded `3` sem explicação
```csharp
if (retryCount > 3) { ... } // What is 3?
```
**Recommendation**:
```csharp
private const int MAX_RETRY_ATTEMPTS = 3;
if (retryCount > MAX_RETRY_ATTEMPTS) { ... }
```
**Severity**: 🟡 Medium - Readability  
**Effort**: 2 min fix

---

### 🟡 MEDIUM-2: Low Test Coverage
**Location**: `ExportServiceTests.cs`  
**Issue**: Only happy path tested, no error cases
**Coverage**: 65% (target >80%)  
**Missing Tests**:
- ExportService_WhenFileWriteFails_ThrowsException
- ExportService_WhenUserNotFound_ReturnsNull
- ExportService_WhenLargeDataset_Paginates  
**Severity**: 🟡 Medium - Quality  
**Effort**: 20 min to add 3 tests

---

## Low Priority / Nits (Optional)

### 💡 LOW-1: Variable Naming
**Location**: `ExportController.cs:23`  
**Issue**: Variable named `temp` - unclear purpose
```csharp
var temp = ProcessExport(data);
```
**Recommendation**: `var exportResult = ProcessExport(data);`  
**Effort**: 1 min

---

## Positive Observations ✅

- ✅ **Good**: Dependency injection usado corretamente
- ✅ **Good**: API endpoint tem integration test
- ✅ **Good**: Logging em pontos chave
- ✅ **Good**: README updated com novas config options

---

## Acceptance Criteria Validation

### AC1: Validate export format
✅ PASS - Code validates format against allowed list  
❌ FAIL - Error message vago ("Invalid format" - should say which formats allowed)

### AC2: Validate file size
✅ PASS - Checks file size < 10MB  
✅ PASS - Returns clear error if too large

### AC3: Validate permissions
❌ FAIL - Missing authorization check (only checks authentication)

**AC Coverage**: 2/3 passing (66%)

---

## Definition of Done Checklist

- [x] Code implemented
- [ ] Unit tests (>80% coverage) - Currently 65%
- [ ] Integration tests - ✅ Present
- [ ] Acceptance criteria validated - ❌ 1/3 failing
- [ ] Documentation updated - ✅ README updated
- [x] No critical bugs - ❌ 2 critical issues found
- [ ] Code merged to main - Blocked by issues

**DoD Status**: ❌ NOT READY - 3 critical + 2 high issues MUST be fixed

---

## Verdict: ❌ REQUEST CHANGES

**Reasoning**:
Code has 2 **CRITICAL** security/stability issues (SQL injection, null reference) que DEVEM ser resolvidos antes de merge. Também tem 2 **HIGH** priority performance/error handling issues que significantly impactam production quality.

**Required Actions**:
1. Fix CRITICAL-1 (SQL injection) - 5 min
2. Fix CRITICAL-2 (null check) - 2 min
3. Fix HIGH-1 (N+1 query) - 10 min
4. Fix HIGH-2 (error handling) - 5 min
5. Add missing AC3 (authorization check) - 15 min
6. Increase test coverage to >80% - 20 min

**Estimated Fix Time**: ~1 hour total

**Next Steps**:
1. Developer fixes issues
2. Push updated code
3. Re-request review (Carla will re-review within 4 hours)

---

## Auto-Fix Option

**Offer**: "Eu posso fazer auto-fix dos 4 critical/high issues automaticamente. Quer que eu faça?"

Se developer aceitar:
- Create branch `fix/code-review-ST-042`
- Apply fixes para CRITICAL-1, CRITICAL-2, HIGH-1, HIGH-2
- Commit com message "fix: Code review auto-fixes for ST-042"
- Developer review auto-fixes e merge se correto

---

**Reviewer**: Carla QA  
**Review Time**: 25 minutes  
**Follow-up**: Re-review após fixes (ETA: 4 hours)
```

---

## 🔗 Integration Points

### Prerequisites:
- **dev-story** completed → Story implementation done, code pushed
- **Story file** (ST-XXX.md) → For AC validation
- **Architecture doc** (optional) → For architecture compliance check

### Triggers code-review:
- Developer completa story via `dev-story` workflow
- Developer manually requests review: `avanade-method-bmm-code-review ST-042`

### Next Steps After Review:
1. **If APPROVED** → Merge to main, update story status to `completed`
2. **If REQUEST CHANGES** → Developer fixes, re-requests review
3. **Auto-fix option** → Reviewer pode fazer fixes automáticos se developer aceitar

### Updates:
- **sprint-status.yaml** → Story status updated to `in-review` during review, `completed` when approved
- **Story file** → Status field updated

---

## ✅ Best Practices

### DO:
- ✅ **Be thorough** - SEMPRE encontrar 3-10 issues (se não achou, procure mais)
- ✅ **Be specific** - Point to exact lines, show before/after code
- ✅ **Explain impact** - "Why this matters" (crash risk, security, performance)
- ✅ **Offer solutions** - Não só criticar, mostrar como fix
- ✅ **Prioritize issues** - Critical vs High vs Medium
- ✅ **Test coverage** - Checar se testes são bons, não só coverage %
- ✅ **Security mindset** - Assume inputs são malicious

### DON'T:
- ❌ **Não aprove sem issues** - Se não achou nada, você não procurou suficiente
- ❌ **Não seja vago** - "This looks wrong" → específico "Null check missing line 45"
- ❌ **Não ignore tests** - "Code works" não é suficiente, precisa tests
- ❌ **Não skip security** - SQL injection, XSS são CRITICAL
- ❌ **Não accept "works on my machine"** - Edge cases, errors, performance matters

---

## 🚨 Common Pitfalls

### Pitfall 1: **Rubber Stamp Review ("LGTM")**
**Sintoma**: Review rápido em 2 minutos, "Looks good to me!", approve  
**Problema**: Bugs vão para production não detectados  
**Solução**: **Adversarial mindset** - assume code tem bugs, procure até encontrar

### Pitfall 2: **Only Happy Path Review**
**Sintoma**: Checa se código funciona, não checa error handling  
**Problema**: Production errors, crashes, data corruption  
**Solução**: Perguntar "O que acontece se isso falhar?" para cada operação

### Pitfall 3: **Ignoring Tests**
**Sintoma**: "Code coverage 65%, approve anyway"  
**Problema**: Bugs não detectados por tests, regressions no futuro  
**Solução**: Test coverage >80% é REQUIREMENT, não suggestion

### Pitfall 4: **Missing Security Review**
**Sintoma**: Review foca em code style, ignora SQL injection  
**Problema**: Security vulnerabilities em production  
**Solução**: Security checklist é OBRIGATÓRIO (SQL injection, XSS, auth, secrets)

---

## 💡 Examples

### Example: Good Review Finding

**GOOD** ✅:
```markdown
### ❌ CRITICAL-1: SQL Injection Vulnerability
**Location**: `UserService.cs:45`  
**Issue**: User input concatenated into SQL query without sanitization
```csharp
// VULNERABLE CODE
var query = $"SELECT * FROM Users WHERE Username='{username}'";
// If username = "admin' OR '1'='1", returns all users!
```
**Impact**: 
- Severity: 🔴 CRITICAL
- Attacker pode bypass authentication, access all user data, delete records
- OWASP Top 10 #1 vulnerability

**Fix** (5 min):
```csharp
// Use parameterized query
var query = "SELECT * FROM Users WHERE Username=@username";
cmd.Parameters.AddWithValue("@username", username);
```

**Testing**: Add test `UserService_SqlInjectionAttempt_IsBlocked()`
```

**BAD** ❌:
```markdown
### Issue: Database query looks wrong
Location: UserService.cs  
Fix: Maybe use better query
```
**Por que BAD**: Vago (qual linha?), não explica problema (SQL injection?), não mostra como fix, não quantifica severity

---

## 📖 References

- **Avanade Method Workflow Path**: `_avanade-method/bmm/workflows/4-implementation/code-review/`
- **Workflow Manifest Entry**: `workflow-manifest.csv` line 20
- **Command**: `avanade-method-bmm-code-review ST-042` (specify story ID)
- **Owner Agent**: Carla QA

**Related Artifacts**:
- ${AVANADE_TASK_CLEAN_CODE} - Clean code principles checklist
- ${AVANADE_TASK_DEBUGGING_GUIDE} - Debugging best practices
- ${AVANADE_TEST_PLAN_TEMPLATE} - Test planning reference

**Related Workflows**:
- `dev-story` → Implements story (triggers code-review)
- `sprint-status` → Tracks story status (in-review, completed)
- `correct-course` → If major changes needed after review

---