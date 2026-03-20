# ============================================================
# D365 Plugin Development Workflow - Avanade Method v4.29.0
# ============================================================
# Rota para: Tiago Dev
# Trigger: Story tipo "Plugin" pronta para desenvolvimento

---
id: d365-plugin-dev
name: "D365 Plugin Development"
version: "1.0"
owner: tiago-dev
output: "[story file updates + code files]"

## Pre-Conditions

- [ ] Story tipo "Plugin" com status "Ready"
- [ ] plugin_spec preenchida na story
- [ ] unit_test_spec preenchida na story
- [ ] Architecture review aprovada (Wilson)

---

## Workflow Steps

### Step 1: Setup & Scaffolding

**Verificar projeto existe**:
```
Estrutura esperada:
src/
├── FTD.Plugins/
│   ├── FTD.Plugins.csproj
│   ├── Base/
│   │   └── PluginBase.cs          # Base class com ITracingService, IOrganizationService
│   ├── [Entity]/
│   │   └── [EntityMessageStage].cs # Plugin class
│   └── Helpers/
│       └── [Shared helpers]
├── FTD.Plugins.Tests/
│   ├── FTD.Plugins.Tests.csproj   # FakeXrmEasy + xUnit
│   └── [Entity]/
│       └── [EntityMessageStage]Tests.cs
```

**Se projeto não existe**: Criar estrutura base com PluginBase.

---

### Step 2: Implement Plugin

**Pattern obrigatório**:

```csharp
public class EntityMessageStage : PluginBase
{
    // 1. VALIDATE CONTEXT
    protected override void ExecutePlugin(ILocalPluginContext context)
    {
        var tracingService = context.TracingService;
        tracingService.Trace("Plugin started: {0}", nameof(EntityMessageStage));

        // Validate depth (avoid infinite loops)
        if (context.PluginExecutionContext.Depth > 1) return;

        // Validate message and entity
        // ...

        // 2. EXTRACT DATA
        var target = context.PluginExecutionContext.InputParameters["Target"] as Entity;
        // Pre/Post images se configurados

        // 3. EXECUTE BUSINESS LOGIC
        try
        {
            // Business logic here
        }
        catch (Exception ex) when (ex is not InvalidPluginExecutionException)
        {
            // 4. HANDLE ERRORS
            tracingService.Trace("Error: {0}", ex.ToString());
            throw new InvalidPluginExecutionException(
                "Mensagem amigável para o usuário", ex);
        }
    }
}
```

**Regras obrigatórias**:
- ✅ ITracingService para TODOS os logs (nunca Console.Write)
- ✅ InvalidPluginExecutionException para erros de negócio
- ✅ Depth check para evitar loops infinitos
- ✅ Early-bound entities (classes geradas via CrmSvcUtil/pac modelbuilder)
- ✅ Sem hard-coded GUIDs ou magic strings (usar constantes ou config)
- ✅ Dispose de OrganizationServiceProxy apenas se criado manualmente
- ❌ Nunca usar Thread, async/await em plugins Dataverse
- ❌ Nunca acessar filesystem, rede ou recursos externos diretamente
- ❌ Nunca usar static state (plugins são stateless)

---

### Step 3: Write Unit Tests

**Framework**: FakeXrmEasy + xUnit

**Pattern obrigatório**:
```csharp
public class EntityMessageStageTests
{
    private readonly XrmFakedContext _context;
    private readonly IOrganizationService _service;

    public EntityMessageStageTests()
    {
        _context = new XrmFakedContext();
        _service = _context.GetOrganizationService();
    }

    [Fact]
    public void Should_DoExpectedBehavior_When_ValidInput()
    {
        // Arrange
        // ...setup entities, relationships

        // Act
        _context.ExecutePluginWith<EntityMessageStage>(inputParameters);

        // Assert
        // ...verify expected changes
    }

    [Fact]
    public void Should_ThrowError_When_InvalidInput()
    {
        // Arrange + Act + Assert
        Assert.Throws<InvalidPluginExecutionException>(() =>
            _context.ExecutePluginWith<EntityMessageStage>(inputParameters));
    }
}
```

**Cobertura mínima**: 80% (conforme d365-config.yaml)

---

### Step 4: Plugin Registration (Documentation)

**Documentar no Dev Agent Record da story**:

```yaml
plugin_registration:
  assembly: "FTD.Plugins"
  step:
    message: "[Create/Update/Delete]"
    primary_entity: "[logical_name]"
    stage: "[PreValidation/PreOperation/PostOperation]"
    execution_mode: "[Synchronous/Asynchronous]"
    filtering_attributes: ["attr1", "attr2"]
    pre_image:
      name: "PreImage"
      attributes: ["attr1", "attr2"]
    post_image:
      name: "PostImage"
      attributes: ["attr1", "attr2"]
```

---

### Step 5: Quality Gate

**Checklist Plugin**:
- [ ] Plugin segue pattern Validate → Extract → Execute → Handle
- [ ] ITracingService utilizado para logging
- [ ] InvalidPluginExecutionException para erros de negócio
- [ ] Depth check implementado
- [ ] Sem hard-coded GUIDs
- [ ] Sem async/await ou Thread
- [ ] Sem acesso a filesystem/rede
- [ ] Unit tests escritos (FakeXrmEasy)
- [ ] Cobertura ≥ 80%
- [ ] Plugin registration documentado
- [ ] Dev Agent Record atualizado na story

**Handoff**: → Carla QA (Code Review)

---

## D365 PCF Development (Alt Flow)

### PCF-specific steps:

```
1. pac pcf init --namespace FTD --name [ControlName] --template field
2. Implement IInputs/IOutputs in index.ts
3. React component if complex UI
4. Jest tests (min 70% coverage)
5. pac pcf push --publisher-prefix ftd
6. Document manifest.xml properties
```

**Pattern PCF lifecycle**:
```typescript
class FTDControl implements ComponentFramework.StandardControl<IInputs, IOutputs> {
    init(context, notifyOutputChanged, state, container) { /* setup */ }
    updateView(context) { /* render */ }
    getOutputs() { /* return bound values */ }
    destroy() { /* cleanup */ }
}
```
