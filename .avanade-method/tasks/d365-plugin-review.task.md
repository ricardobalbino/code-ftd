# ============================================================
# D365 Plugin Review Task - Avanade Method v4.29.0
# ============================================================
# Owner: Carla QA
# Atomic task for reviewing a single plugin implementation

---
id: d365-plugin-review
name: "D365 Plugin Adversarial Review"
owner: carla-qa

## Purpose

Revisão adversarial de UM plugin C# Dataverse, verificando qualidade,
segurança, performance e compliance com standards.

## Input Required

- Plugin source code (.cs file)
- Plugin registration details (entity, message, stage, mode)
- Unit test file (.cs file)
- Story/AC que o plugin implementa

## Review Dimensions (8)

### 1. CORRECTNESS
- [ ] Plugin implementa TODOS os ACs da story
- [ ] Lógica de negócio correta para cenários normais
- [ ] Edge cases tratados (null, empty, boundary values)
- [ ] Cascade behavior adequado (create/update/delete)

### 2. SECURITY
- [ ] Sem credenciais hardcoded
- [ ] Input validation em todos os parâmetros
- [ ] Sem exposição de dados sensíveis em trace
- [ ] Sem SQL/FetchXML injection (se dinâmico)
- [ ] Authorization check se necessário

### 3. PERFORMANCE
- [ ] Sem query em loop (N+1)
- [ ] ColumnSet especificado (nunca AllColumns)
- [ ] ExecuteMultiple para batch (max 1000)
- [ ] Pre/Post images apenas com atributos necessários
- [ ] Execution target < 2s

### 4. RELIABILITY
- [ ] Depth check (> 1 return)
- [ ] Error handling com InvalidPluginExecutionException
- [ ] ITracingService para logging
- [ ] Sem async/await ou Thread
- [ ] Sem static state

### 5. MAINTAINABILITY
- [ ] Código segue naming conventions
- [ ] Single Responsibility
- [ ] Early-bound entities
- [ ] Constantes para GUIDs e option sets
- [ ] Código autoexplicativo (minimal comments needed)

### 6. TESTABILITY
- [ ] FakeXrmEasy unit tests existem
- [ ] Happy path coberto
- [ ] Error paths cobertos
- [ ] Cobertura ≥ 80%

### 7. STANDARDS COMPLIANCE
- [ ] Segue PluginBase pattern
- [ ] Namespace correto (FTD.Plugins.[Module])
- [ ] Registration documentado
- [ ] Filtering attributes configurados

### 8. OPERATIONAL
- [ ] Solution Checker clean
- [ ] Registration pode ser automatizada (via spkl/pac)
- [ ] Tracing adequado para troubleshooting em prod

## Output

```yaml
review_result:
  verdict: "[APPROVED / APPROVED_WITH_COMMENTS / REQUEST_CHANGES / REJECTED]"
  scores:
    correctness: "[1-5]"
    security: "[1-5]"
    performance: "[1-5]"
    reliability: "[1-5]"
    maintainability: "[1-5]"
    testability: "[1-5]"
    standards: "[1-5]"
    operational: "[1-5]"
  issues:
    critical: ["[issue]"]
    high: ["[issue]"]
    medium: ["[issue]"]
    low: ["[suggestion]"]
  summary: "[Resumo em 2-3 frases]"
```
