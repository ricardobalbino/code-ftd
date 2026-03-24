# Padrões de Desenvolvimento — Avanade BCA v3

> [← Voltar ao Início](./Home.md)

**Referência**: `.github/instructions/avanade-bca-guidelines.instructions.md`  
**Inventário completo BCA**: `docs/diretriz-avanade-inventory.md` (197 docs)

---

## 1. Princípios Gerais

1. **Early Bound obrigatório** — nunca magic strings para entidades e campos
2. **Shift-Left Testing** — testes escritos junto com o código, nunca depois
3. **Plugin < 2 segundos** — execução total sob carga de pico
4. **TypeScript obrigatório** — zero JavaScript sem tipagem em código novo
5. **Security by design** — sem credenciais hardcoded, sem PII em logs
6. **Prefixo `ftd_`** — em TUDO que for customizado
7. **Nunca editar solução Default** — sempre soluções segmentadas

---

## 2. Backend — Plugins C#

### 2.1 Padrão Arquitetural: Plugin → Service → Repository

```csharp
// 1. Plugin: entry point, apenas orquestra
[CrmPluginRegistration(
    MessageNameEnum.Create,
    "ftd_proposta",
    StageEnum.PreOperation,
    ExecutionModeEnum.Synchronous,
    FilteringAttributes: "ftd_status_proposta,ftd_nivel_alcada",
    StepName: "FTD.Plugins.Proposta.PreCreate - Validar Proposta")]
public class PreCreateValidarProposta : PluginBase<Entity>
{
    public PreCreateValidarProposta()
    {
        // DI via Unity container
    }

    protected override void ExecuteBusinessLogic(ILocalPluginContext localContext)
    {
        var target = localContext.PluginExecutionContext
            .InputParameters["Target"] as Entity;

        // 2. Delega para Service (lógica de negócio)
        var service = Container.Resolve<IPropostaService>();
        service.ValidarCriacao(target, localContext.OrganizationService,
            localContext.TracingService);
    }
}

// 2. Service: lógica de negócio (nunca no plugin)
public class PropostaService : IPropostaService
{
    private readonly IPropostaRepository _repository;
    public PropostaService(IPropostaRepository repository) => _repository = repository;

    public void ValidarCriacao(Entity proposta, IOrganizationService service,
        ITracingService trace)
    {
        trace.Trace("PropostaService.ValidarCriacao: iniciando");
        // ... regras de negócio
    }
}

// 3. Repository: acesso a dados
public class PropostaRepository : IPropostaRepository
{
    public Entity GetOportunidade(Guid id, IOrganizationService service)
    {
        // Early Bound + ColumnSet específico (NUNCA ColumnSet(true))
        var cols = new ColumnSet(
            Opportunity.Fields.Name,
            Opportunity.Fields.ftd_safra);
        return service.Retrieve(Opportunity.EntityLogicalName, id, cols);
    }
}
```

### 2.2 Stages de Plugin — Quando Usar

| Stage | Quando Usar | Exemplos FTD |
|-------|-------------|--------------|
| **Pre-Validation (10)** | Bloqueia ANTES de tudo | Validar CNPJ duplicado |
| **Pre-Operation (20)** | Modifica dados antes de persistir | Calcular nível de alçada |
| **Post-Operation Sync (40)** | Após salvar, dentro da transação | Criar vínculo proposta anterior/posterior |
| **Post-Operation Async (40)** | Notificações, logs (não crítico) | Log em ftd_log_integracao |

### 2.3 Regras Obrigatórias

```csharp
// ✅ CORRETO: Depth check
if (context.Depth > 1) return;

// ✅ CORRETO: Tracing em tudo
trace.Trace($"[{nameof(PreCreateValidarProposta)}] Target: {target.Id}");

// ✅ CORRETO: Exceção de negócio
throw new InvalidPluginExecutionException(
    OperationStatus.Failed,
    "CNPJ duplicado. Operação bloqueada.");

// ✅ CORRETO: Filtering attributes
// FilteringAttributes: "ftd_status_proposta,ftd_nivel_alcada"

// ✅ CORRETO: ColumnSet específico
var cols = new ColumnSet(Account.Fields.ftd_cnpj, Account.Fields.Name);

// ❌ PROIBIDO: ColumnSet(true)
var cols = new ColumnSet(true); // NUNCA!

// ❌ PROIBIDO: GUID hardcoded
var guid = new Guid("a1b2c3d4-..."); // NUNCA!

// ✅ CORRETO: Constante nomeada
public static class FtdOptionSets
{
    public static class TipoProposta
    {
        public const int ContratoNovo = 100000000;
        public const int Aditivo = 100000001;
    }
}
```

### 2.4 Regra de Débounce (Plugin vs Azure Function)

```
≤ 50 produtos → Plugin síncrono (Stage 20/40)
> 50 produtos → Azure Function via Service Bus (assíncrono)
```

### 2.5 Testes de Plugin (FakeXrmEasy)

```csharp
[TestClass]
public class PreCreateValidarPropostaTests
{
    [TestMethod]
    public void QuandoCNPJDuplicado_DeveBloquearOperacao()
    {
        // Arrange
        var ctx = new XrmFakedContext();
        ctx.Initialize(new List<Entity>
        {
            new Account { Id = Guid.NewGuid(), ["ftd_cnpj"] = "12.345.678/0001-99" }
        });
        var plugin = new PreCreateValidarProposta();
        var target = new Account { ["ftd_cnpj"] = "12.345.678/0001-99" };

        // Act + Assert
        Assert.ThrowsException<InvalidPluginExecutionException>(() =>
            ctx.ExecutePluginWithTarget(plugin, target));
    }
}
// Cobertura mínima: 80%
```

---

## 3. Frontend — PCF Controls (TypeScript)

### 3.1 Padrão: Contract / Controller

```typescript
// contract.ts — Singleton, entry points, despacha para controller
export namespace PropostaForm {
    export function onLoad(ctx: Xrm.Events.EventContext): void {
        const controller = new PropostaFormController(ctx.getFormContext());
        controller.initialize();
    }
    export function onSave(ctx: Xrm.Events.SaveEventContext): void {
        const controller = new PropostaFormController(ctx.getFormContext());
        controller.onSave(ctx);
    }
}

// controller.ts — Lógica de negócio do formulário
export class PropostaFormController {
    private readonly _formContext: Xrm.FormContext;

    constructor(formContext: Xrm.FormContext) {
        this._formContext = formContext;
    }

    public initialize(): void {
        this.configurarRegraVisibilidade();
        this.carregarProdutos();
    }

    private configurarRegraVisibilidade(): void {
        const nivel = this._formContext
            .getAttribute("ftd_nivel_alcada")?.getValue() ?? 0;
        this._formContext
            .getControl("ftd_justificativa_consultor")
            ?.setVisible(nivel > 1);
    }
}
```

### 3.2 Regras Obrigatórias TypeScript

```typescript
// ✅ CORRETO: === sempre (nunca ==)
if (value === null) { }

// ✅ CORRETO: JSON (nunca XML para dados)
const data = JSON.parse(response);

// ✅ CORRETO: verificar métodos existentes antes de criar novos
// ✅ CORRETO: Fluent Rules para reatividade
// ✅ CORRETO: OData Query Builder em vez de fetchXml manual

// ❌ PROIBIDO: == (loose equality)
if (value == null) { } // NUNCA!
```

### 3.3 Cache — ESSENCIAL

> Dataverse Web API retorna `no-cache` por padrão — implementar cache client-side!

```typescript
// Usar DefaultCache + CacheItem
const cacheKey = sha256(JSON.stringify(request));
const cached = DefaultCache.get<Product[]>(cacheKey);
if (cached) return cached;
const result = await fetchProducts(request);
DefaultCache.set(cacheKey, result, CacheItem.minutes(30));
return result;
```

---

## 4. Naming Conventions

### Entidades e Campos
| Item | Padrão | Exemplo |
|------|--------|---------|
| Campo customizado | `ftd_` + snake_case | `ftd_nivel_alcada` |
| Entidade customizada | `ftd_` + nome | `ftd_proposta` |
| OptionSet | `ftd_` + nome | `ftd_tipo_proposta` |
| Plugin class | PascalCase + Stage + Ação | `PreCreateValidarCNPJ` |
| Service class | PascalCase + Service | `PropostaService` |
| Repository class | PascalCase + Repository | `PropostaRepository` |

### Arquivos TypeScript
| Tipo | Padrão | Exemplo |
|------|--------|---------|
| Form contract | `[entity].[form].form.contract.ts` | `proposta.main.form.contract.ts` |
| Form controller | `[entity].[form].form.controller.ts` | `proposta.main.form.controller.ts` |
| Ribbon contract | `[entity].ribbon.contract.ts` | `proposta.ribbon.contract.ts` |

---

## 5. Testes

### Backend: MSTest + NSubstitute (padrão AAA)

```csharp
[TestMethod]
public void NomeClasse_Metodo_Should_Comportamento_When_Estado()
{
    // Arrange — setup
    var mockRepo = Substitute.For<IPropostaRepository>();
    var service = new PropostaService(mockRepo);

    // Act — executa
    var result = service.CalcularAlcada(proposta);

    // Assert — verifica
    Assert.AreEqual(3, result.Nivel);
}
```

### Frontend: Jest + Should.js

```typescript
describe("PropostaFormController", () => {
    it("deve ocultar justificativa quando alçada = 1", () => {
        const ctx = new DynamicsContextMock();
        ctx.setAttribute("ftd_nivel_alcada", 1);
        const controller = new PropostaFormController(ctx.formContext);
        controller.initialize();
        ctx.getControl("ftd_justificativa_consultor").isVisible
            .should.equal(false);
    });
});
```

### Cobertura Mínima
- C# Plugins: **80%** de cobertura
- TypeScript: acordo definido por sprint

---

## 6. Pipelines CI/CD

| Pipeline | Trigger | Função |
|----------|---------|--------|
| `build-guard` | Auto (CI — cada commit/PR) | Build frontend + backend + TODOS os unit tests |
| `deploy-code` | Auto (merge PR → main) | Deploy artifacts para Customization Master |
| `deploy` | **MANUAL** | Full deploy para upper environments (solutions + data) |
| `export-unpack` | Opcional | Exporta/desempacota solutions → cria PR automático |

### Regras de Pipeline
- Deploy para upper environments é **SEMPRE MANUAL** — nunca automatizar
- Guard build DEVE passar antes de qualquer PR merge
- Secrets via Azure Key Vault linked Library (**NUNCA** no repositório)

---

## 7. Fluxo de Trabalho Diário

```
1. git pull latest (branch dev)
2. GenerateEarlyBound (atualizar tipos)
3. Develop (plugin/TypeScript)
4. LocalDeployment (CLI: deploy local para D365 Dev)
5. Verificar no CRM Dev
6. Customizar (se necessário no make.powerapps.com)
7. UnpackSolution (CLI: exporta e desempacota)
8. git commit -m "feat: #workitem - descrição"
9. PR com título descritivo + work item linkado
10. Guard build auto-trigger (CI)
11. Deploy manual para OAT (pipeline)
12. QA Sign-off
13. Deploy manual para PROD + GMUD
```

---

## 8. Code Review Checklist

- [ ] Git: branches limpas, push frequente
- [ ] Ambientes dev: componentes de teste removidos após uso
- [ ] Coding: formatação correta, naming conventions, sem magic numbers
- [ ] C#: zero warnings, StyleCop clean
- [ ] TypeScript: ESLint clean, `===` sempre
- [ ] Unit tests: padrão AAA, naming convention, cobertura ≥ threshold
- [ ] Performance: sem roundtrips desnecessários, cache aplicado
- [ ] Security: sem credenciais hardcoded, sem PII em logs
- [ ] Early Bound: tipos gerados e utilizados

---

*Referências: [docs/arquitetura/padroes-desenvolvimento.md](../docs/arquitetura/padroes-desenvolvimento.md) | [.github/instructions/avanade-bca-guidelines.instructions.md](../.github/instructions/avanade-bca-guidelines.instructions.md)*
