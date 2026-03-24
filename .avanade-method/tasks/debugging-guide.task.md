## Objetivo
Processo estruturado para diagnosticar e corrigir bugs eficientemente.

---

## ?? Debugging Workflow

### 1. Reproduce (Reproduzir)
**Critério**: Confirmar bug é real e reproduzível

**Checklist**:
- [ ] **Consistência**: Consegue reproduzir 100% das vezes?
- [ ] **Steps**: Documentar passos exatos (Given/When/Then)
- [ ] **Environment**: Dev, Staging, Produção? Versão do app?
- [ ] **Data**: Massa de dados específica causa o bug?
- [ ] **User/Permissions**: Bug ocorre para todos usuários ou role específico?

**Template de Reprodução**:
```
BUG: Login falha com erro 500

ENVIRONMENT: Staging (v2.3.1)
USER: Role = "Admin"
BROWSER: Chrome 120

STEPS:
1. Navegar para /login
2. Preencher email: admin@test.com
3. Preencher senha: Admin@123
4. Clicar "Entrar"

EXPECTED: Redirecionar para /dashboard
ACTUAL: Erro 500 "Internal Server Error"

FREQUENCY: 10/10 tentativas
```

**Se não consegue reproduzir**:
- ? **Não é bug** ? pode ser ambiente específico, cache, etc.
- ?? **Coletar mais dados**: Logs, screenshot, video, network trace

---

### 2. Isolate (Isolar)
**Critério**: Identificar componente/módulo/função com problema

**Técnicas**:

#### ?? Binary Search (Bisection)
Dividir e conquistar - desabilitar metade do código até isolar:
```
Sistema com 10 módulos ? testar com 5 desabilitados
  ? Bug ainda ocorre ? problema está nos 5 ativos
    ? Desabilitar 2-3 desses ? repetir até isolar 1 módulo
```

#### ?? Component Isolation
Testar componente isoladamente:
```
// Teste isolado do componente suspeito
const result = suspectFunction(testInput);
console.log(result); // Comportamento esperado?
```

#### ?? Hypothesis Testing
Formular hipóteses e testar:
```
Hipótese 1: "Bug está no backend (API)"
  Teste: Chamar API diretamente via Postman
  Resultado: API retorna 500 ? ? Confirmada

Hipótese 2: "Bug está no database query"
  Teste: Executar query diretamente no SQL
  Resultado: Query retorna dados corretos ? ? Refutada
```

**Output desta fase**: Arquivo/função/linha suspeita

---

### 3. Analyze (Analisar)
**Critério**: Entender **por que** bug está acontecendo

**Ferramentas**:

#### ?? Stack Trace
Ler erro de baixo para cima (call stack):
```
Error: Cannot read property 'name' of undefined
  at getUserName (user.service.ts:45)    ? origem do erro
  at processUser (user.controller.ts:23) ? chamou getUserName
  at handleRequest (app.ts:12)          ? chamou processUser
```

**Foco**: Primeira linha do **seu código** (não de libs)

#### ?? Debugger (Breakpoints)
Pausar execução e inspecionar estado:
```typescript
function calculateDiscount(user: User, order: Order) {
  debugger; // ? código pausa aqui
  
  const discount = user.isPremium ? 0.2 : 0;
  const total = order.total * (1 - discount);
  
  return total;
}
```

**Inspecionar**:
- Valores de variáveis (`user`, `order`, `discount`)
- Call stack (quem chamou esta função)
- Watch expressions (condições customizadas)

#### ?? Logging Estratégico
Adicionar logs temporários para rastrear fluxo:
```typescript
function processOrder(order: Order) {
  console.log('[DEBUG] processOrder START', { orderId: order.id });
  
  const validatedOrder = validateOrder(order);
  console.log('[DEBUG] After validation', { validatedOrder });
  
  const savedOrder = saveOrder(validatedOrder);
  console.log('[DEBUG] After save', { savedOrder });
  
  return savedOrder;
}
```

**Boas práticas**:
- Prefix: `[DEBUG]`, `[TRACE]` para fácil remoção
- Dados relevantes: IDs, estados, valores críticos
- Remover após fix (não commitar logs de debug)

#### ?? Network Inspector
Para bugs de API/integração:
```
Request:
  POST /api/orders
  Headers: { Authorization: "Bearer abc123" }
  Body: { userId: 42, items: [...] }

Response:
  Status: 500
  Body: { error: "User not found" } ? aha! userId inválido
```

---

### 4. Hypothesis (Hipótese)
**Critério**: Teorizar causa raiz baseado em análise

**Template de Hipótese**:
```
HIPÓTESE: [Descrição concisa da causa raiz]
EVIDÊNCIA: [Dados que suportam hipótese]
TESTE: [Como validar hipótese]
```

**Exemplo**:
```
HIPÓTESE: User.isPremium está undefined causando cálculo errado
EVIDÊNCIA: 
  - Stack trace aponta para user.isPremium
  - Logs mostram user = { id: 42, name: "John" } (sem isPremium)
  - Database schema mostra coluna "isPremium" foi adicionada recentemente
TESTE: 
  - Adicionar default value na migration
  - Ou adicionar fallback no código: user.isPremium ?? false
```

---

### 5. Fix (Corrigir)
**Critério**: Implementar correção mínima e testável

**Princípios**:
- [ ] **Minimal change**: Menor alteração que resolve o problema
- [ ] **Root cause**: Corrige causa raiz, não sintoma
- [ ] **Backward compatible**: Não quebra código existente
- [ ] **Tested**: Teste automatizado valida fix

**Exemplo de Fix**:
```typescript
// ANTES (bug):
function calculateDiscount(user: User) {
  return user.isPremium ? 0.2 : 0; // ? crash se isPremium undefined
}

// DEPOIS (fix):
function calculateDiscount(user: User) {
  return user.isPremium === true ? 0.2 : 0; // ? safe
}

// OU com fallback explícito:
function calculateDiscount(user: User) {
  const isPremium = user.isPremium ?? false; // ? default value
  return isPremium ? 0.2 : 0;
}
```

---

### 6. Test (Testar)
**Critério**: Validar fix e prevenir regressão

**Checklist**:
- [ ] **Bug original**: Reprodução original agora funciona?
- [ ] **Edge cases**: Outros cenários relacionados ainda funcionam?
- [ ] **Regression test**: Teste automatizado adicionado
- [ ] **Code review**: Outra pessoa validou fix

**Regression Test**:
```typescript
describe('calculateDiscount', () => {
  it('should return 0 when user.isPremium is undefined', () => {
    const user = { id: 1, name: 'John' }; // sem isPremium
    
    const discount = calculateDiscount(user);
    
    expect(discount).toBe(0); // ? não crashea
  });

  it('should return 0.2 when user.isPremium is true', () => {
    const user = { id: 1, name: 'Jane', isPremium: true };
    
    const discount = calculateDiscount(user);
    
    expect(discount).toBe(0.2);
  });
});
```

---

### 7. Document (Documentar)
**Critério**: Registrar bug, causa raiz, e solução para aprendizado

**Template de Bug Report**:
```markdown
## BUG-123: Login falha com erro 500 para novos usuários

### Descrição
Usuários criados após 2024-01-15 não conseguem fazer login.

### Root Cause
Migration adicionou coluna `isPremium` mas não populou 
usuários existentes (NULL). Código assumia boolean.

### Fix
- Adicionado default value `false` na migration
- Código agora faz fallback: `user.isPremium ?? false`
- Regression test adicionado

### Prevention
- Code review checklist: validar nullable fields
- Database migrations: sempre incluir backfill para dados existentes

### Links
- PR: #456
- Commit: abc123
- Production Incident: INC-789
```

**Onde documentar**:
- **Commit message**: Detalhado (não só "fix bug")
- **PR description**: Root cause + fix + tests
- **Wiki/Knowledge Base**: Bugs complexos/recorrentes
- **Memory**: ${AVANADE_MEMORY_DEV_TIAGO} (padrões de bugs)

---

## ?? Debugging Anti-Patterns (Evitar)

### ? Random Changes ("Shotgun Debugging")
Mudar código aleatoriamente esperando que bug desapareça.

**Problema**: Não entende causa raiz, pode introduzir novos bugs.

**Solução**: Seguir workflow sistemático (Reproduce ? Isolate ? Analyze)

---

### ? Console.log Everywhere
Poluir código com centenas de logs.

**Problema**: Dificulta leitura, pode gerar noise em produção.

**Solução**: Usar debugger (breakpoints) ou logging estratégico temporário.

---

### ? Cargo Cult Fixes
Copiar solução do StackOverflow sem entender.

**Problema**: Fix funciona mas você não sabe por quê, pode quebrar depois.

**Solução**: Entender **por que** solução funciona antes de aplicar.

---

### ? Quick & Dirty Patch
Adicionar if/else para esconder sintoma ao invés de corrigir causa raiz.

**Problema**: Bug pode reaparecer em outro contexto.

**Solução**: Identificar e corrigir causa raiz.

---

## ?? Ferramentas por Categoria

### Frontend (Browser)
- **Chrome DevTools**: Breakpoints, network, console
- **React DevTools**: Component state/props
- **Vue DevTools**: Vuex state, component hierarchy

### Backend (Node.js/C#)
- **VS Code Debugger**: Breakpoints, watch, call stack
- **Postman**: API testing
- **SQL Client**: Database query testing

### Logging/Monitoring
- **Application Insights**: Distributed tracing, telemetria
- **Sentry**: Error tracking, stack traces
- **ELK Stack**: Log aggregation

### Performance
- **Chrome Lighthouse**: Performance profiling
- **k6**: Load testing
- **SQL Query Analyzer**: Database performance

---

## ?? Debugging Checklist (Quick Reference)

- [ ] **Reproduce**: Bug é consistente? Steps documentados?
- [ ] **Isolate**: Qual componente/função está quebrando?
- [ ] **Analyze**: Por que está quebrando? (stack trace, debugger, logs)
- [ ] **Hypothesis**: Qual é a causa raiz?
- [ ] **Fix**: Correção mínima que resolve root cause
- [ ] **Test**: Regression test adicionado, fix validado
- [ ] **Document**: Root cause e solução documentados

---

## ?? Integração com Metodologia Avanade

- **Trigger**: Bug reportado (Jira, PagerDuty, user feedback)
- **Output**: Fix + Regression test + Documentation
- **Quality**: ${AVANADE_TASK_CODE_REVIEW}, ${AVANADE_TASK_TEST_COVERAGE}
- **Memória**: ${AVANADE_MEMORY_DEV_TIAGO} (patterns de bugs, root causes)
