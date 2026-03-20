## Objetivo
Validar código contra princípios de Clean Code, SOLID, e best practices.

---

## 💎 Clean Code Principles

### 1. Meaningful Names (Nomes Significativos)
**Critério**: Nomes revelam intenção, evitam desinformação

**Checklist**:
- [ ] **Classes**: Substantivos (User, OrderService, ProductRepository)
- [ ] **Methods**: Verbos (calculateTotal, sendEmail, validateInput)
- [ ] **Booleans**: Predicados (isActive, hasPermission, canEdit)
- [ ] **Evita**:  - Abreviações (`usr` → `user`)
  - Nomes genéricos (`data`, `info`, `temp`)
  - Números mágicos (`status === 2` → `status === OrderStatus.SHIPPED`)

**Exemplos**:
```typescript
❌ Ruim:
function d(n: number): number { return n * 365; }

✅ Bom:
function daysToYears(days: number): number { 
  return days / DAYS_PER_YEAR; 
}
```

---

### 2. Functions (Funções Pequenas e Focadas)
**Critério**: Uma função faz uma coisa e faz bem

**Checklist**:
- [ ] **Tamanho**: <20 linhas (ideal), máx 50 linhas
- [ ] **Single Responsibility**: Faz apenas 1 coisa
- [ ] **Nível de abstração**: Consistente (não mistura alto e baixo nível)
- [ ] **Parâmetros**: ≤3 parâmetros (ideal 0-2)
- [ ] **Side effects**: Evita efeitos colaterais escondidos
- [ ] **Command-Query Separation**: Função faz OU retorna, não ambos

**Exemplo**:
```typescript
❌ Ruim (faz demais):
function processOrder(order: Order) {
  validateOrder(order);
  saveOrder(order);
  sendEmail(order.customer.email);
  updateInventory(order.items);
  logAnalytics(order);
}

✅ Bom (orquestra funções menores):
function processOrder(order: Order) {
  const validatedOrder = validateOrder(order);
  const savedOrder = saveOrder(validatedOrder);
  notifyOrderPlaced(savedOrder);
  return savedOrder;
}
```

---

### 3. Comments (Comentários Úteis)
**Critério**: Código se explica, comentários explicam "por quê"

**Checklist**:
- [ ] **Evita comentários óbvios**: Código auto-explicativo
- [ ] **Explica "por quê"**: Decisões, trade-offs, workarounds
- [ ] **TODOs**: Rastreáveis (com ticket/issue)
- [ ] **Warnings**: Consequências críticas
- [ ] **JSDoc/Docstrings**: APIs públicas documentadas

**Exemplos**:
```typescript
❌ Ruim (comentário óbvio):
// Incrementa contador
counter++;

✅ Bom (explica "por quê"):
// Workaround: API retorna null ao invés de array vazio
// Removendo após migração para v2 (ticket JIRA-123)
const items = response.items || [];

✅ Bom (warning crítico):
// ATENÇÃO: Alterar ordem dessas operações causa race condition
// Ver: https://github.com/org/repo/issues/456
await lockResource();
await updateDatabase();
await unlockResource();
```

---

### 4. Error Handling (Tratamento de Erros)
**Critério**: Erros são first-class citizens, não afterthoughts

**Checklist**:
- [ ] **Try-catch**: Em operações críticas (DB, API, I/O)
- [ ] **Error boundaries**: UI não quebra totalmente
- [ ] **Mensagens úteis**: Contexto suficiente para debug
- [ ] **Logging**: Erros logados com severity adequada
- [ ] **Fail fast**: Validação early, falha rápida
- [ ] **Evita silent failures**: Sempre handle ou propague

**Exemplos**:
```typescript
❌ Ruim (ignora erro):
try {
  await updateUser(userId, data);
} catch (e) {
  // ignorado
}

✅ Bom (trata adequadamente):
try {
  await updateUser(userId, data);
} catch (error) {
  logger.error('Failed to update user', { 
    userId, 
    error: error.message, 
    stack: error.stack 
  });
  throw new UserUpdateError('Could not update user profile', { cause: error });
}
```

---

## 🏗️ SOLID Principles

### S - Single Responsibility Principle
**Critério**: Classe tem uma única razão para mudar

```typescript
❌ Ruim (múltiplas responsabilidades):
class User {
  saveToDatabase() { /* DB logic */ }
  sendWelcomeEmail() { /* Email logic */ }
  validateData() { /* Validation logic */ }
}

✅ Bom (separado):
class User { /* apenas dados */ }
class UserRepository { saveToDatabase() {} }
class EmailService { sendWelcomeEmail() {} }
class UserValidator { validate() {} }
```

### O - Open/Closed Principle
**Critério**: Aberto para extensão, fechado para modificação

```typescript
❌ Ruim (precisa modificar para adicionar tipo):
function calculateDiscount(user: User) {
  if (user.type === 'premium') return 0.2;
  if (user.type === 'vip') return 0.3;
}

✅ Bom (extensível via polimorfismo):
interface DiscountStrategy {
  calculate(amount: number): number;
}
class PremiumDiscount implements DiscountStrategy { /* ... */ }
class VIPDiscount implements DiscountStrategy { /* ... */ }
```

### L - Liskov Substitution Principle
**Critério**: Subtipos devem ser substituíveis por seus tipos base

```typescript
❌ Ruim (quebra contrato):
class Bird { fly() { /* ... */ } }
class Penguin extends Bird { 
  fly() { throw new Error('Cannot fly'); } // ❌
}

✅ Bom (design correto):
interface Bird {}
interface FlyingBird extends Bird { fly(); }
class Sparrow implements FlyingBird { fly() { /* ... */ } }
class Penguin implements Bird { swim() { /* ... */ } }
```

### I - Interface Segregation Principle
**Critério**: Interfaces específicas melhor que genéricas

```typescript
❌ Ruim (interface genérica):
interface Worker {
  work();
  eat();
  sleep();
}
class Robot implements Worker {
  work() { /* ok */ }
  eat() { throw new Error(); } // ❌ robô não come
  sleep() { throw new Error(); } // ❌ robô não dorme
}

✅ Bom (interfaces específicas):
interface Workable { work(); }
interface Eatable { eat(); }
interface Sleepable { sleep(); }
class Robot implements Workable { work() {} }
class Human implements Workable, Eatable, Sleepable { /* ... */ }
```

### D - Dependency Inversion Principle
**Critério**: Dependa de abstrações, não de implementações concretas

```typescript
❌ Ruim (dependência concreta):
class UserService {
  private db = new MySQLDatabase(); // ❌ acoplado ao MySQL
}

✅ Bom (dependência abstrata):
interface Database { save(data: any): void; }
class UserService {
  constructor(private db: Database) {} // ✅ aceita qualquer DB
}
```

---

## 🧪 Testing (Testabilidade)

### Checklist
- [ ] **Unit Tests**: Coverage ≥ 80%
- [ ] **Test Pyramid**: Maioria unit, alguns integration, poucos E2E
- [ ] **Fast tests**: Suite completa <10s (unit), <5min (integration)
- [ ] **Isolated**: Testes independentes (ordem não importa)
- [ ] **Determinísticos**: Sem flakiness, sempre mesmo resultado
- [ ] **Readable**: Testes são documentação viva

**Exemplo de Bom Teste**:
```typescript
describe('OrderService.calculateTotal', () => {
  it('should sum item prices when no discount', () => {
    // ARRANGE
    const items = [
      { price: 10, quantity: 2 }, // 20
      { price: 5, quantity: 3 },  // 15
    ];
    
    // ACT
    const total = OrderService.calculateTotal(items);
    
    // ASSERT
    expect(total).toBe(35);
  });

  it('should apply 10% discount for premium users', () => {
    const items = [{ price: 100, quantity: 1 }];
    const user = { type: 'premium' };
    
    const total = OrderService.calculateTotal(items, user);
    
    expect(total).toBe(90); // 100 - 10%
  });
});
```

---

## 🔒 Security

### Checklist
- [ ] **Input validation**: Sanitize/validate todos inputs
- [ ] **SQL Injection**: Usar prepared statements/ORMs
- [ ] **XSS Prevention**: Escape output, CSP headers
- [ ] **Authentication**: Tokens seguros, sessions gerenciadas
- [ ] **Secrets**: Nunca hardcoded, usar env vars ou Key Vault
- [ ] **HTTPS**: Sempre TLS em produção
- [ ] **Dependencies**: Audit regular (npm audit, Snyk)

---

## ⚡ Performance

### Checklist
- [ ] **N+1 Queries**: Evitar loops com queries
- [ ] **Caching**: Dados caros/frequentes em cache
- [ ] **Lazy Loading**: Carregar apenas quando necessário
- [ ] **Pagination**: Não retornar datasets gigantes
- [ ] **Indexes**: Database queries otimizadas
- [ ] **Profiling**: Identificar bottlenecks (APM tools)

---

## 📊 Scoring System

**Pontos por categoria**:
- Meaningful Names: /10
- Functions: /10
- Comments: /10
- Error Handling: /10
- SOLID: /10
- Testing: /10
- Security: /10
- Performance: /10

**Total: /80**

**Interpretação**:
- **70-80**: ✅ Production-Ready Code
- **50-69**: 🟡 Bom, melhorias menores
- **30-49**: 🟠 Precisa refactoring
- **0-29**: 🔴 Não mergeável

---

## 🔗 Integração com Metodologia Avanade

- **Pré-requisito**: Código implementado
- **Uso**: Code review, DoD validation
- **Complementa**: ${AVANADE_TASK_CODE_REVIEW}, ${AVANADE_TASK_TEST_COVERAGE}
- **Memória**: Atualizar ${AVANADE_MEMORY_DEV_TIAGO}