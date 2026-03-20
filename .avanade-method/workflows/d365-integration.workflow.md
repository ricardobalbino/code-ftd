# ============================================================
# D365 Integration Workflow - Avanade Method v4.29.0
# ============================================================
# Rota para: Wilson Architect (design) → Tiago Dev (implementation)
# Trigger: Story tipo "Integration" ou "AzureFunction"

---
id: d365-integration
name: "D365 Integration Development"
version: "1.0"
owner: tiago-dev
co-owner: wilson-architect
output: "[architecture docs + code files + story updates]"

## Workflow Steps

### Step 1: Integration Design (Wilson Architect)

**Decision Framework - Qual pattern usar**:

```
┌─────────────────────────────────────────────────────┐
│ INTEGRATION DECISION TREE                           │
├─────────────────────────────────────────────────────┤
│                                                     │
│ É real-time e simples?                              │
│ ├── SIM → Dataverse Webhook → Azure Function HTTP   │
│ │                                                   │
│ É real-time mas precisa de retry/resilience?        │
│ ├── SIM → Plugin → Service Bus → Azure Function     │
│ │                                                   │
│ É assíncrono / batch?                               │
│ ├── SIM → Azure Function Timer + Dataverse API      │
│ │                                                   │
│ É event-driven (reage a mudanças)?                  │
│ ├── SIM → Event Grid → Azure Function               │
│ │                                                   │
│ É orquestração multi-step?                          │
│ ├── SIM → Power Automate Cloud Flow                 │
│ │                                                   │
│ É import/export de dados em massa?                  │
│ └── SIM → Azure Function + ExecuteMultiple          │
│                                                     │
└─────────────────────────────────────────────────────┘
```

**Output**: ADR documentando decisão de pattern.

---

### Step 2: Azure Function Implementation (Tiago Dev)

**Estrutura projeto**:
```
src/
├── FTD.Functions/
│   ├── FTD.Functions.csproj
│   ├── Program.cs                    # Host builder, DI
│   ├── [Domain]/
│   │   └── [FunctionName].cs         # Function class
│   ├── Services/
│   │   ├── IDataverseService.cs      # Abstraction
│   │   └── DataverseService.cs       # Dataverse SDK wrapper
│   ├── Models/
│   │   └── [DTOs].cs
│   └── Config/
│       └── AppSettings.cs
├── FTD.Functions.Tests/
│   ├── FTD.Functions.Tests.csproj
│   └── [Domain]/
│       └── [FunctionName]Tests.cs
```

**Pattern Azure Function**:
```csharp
public class FunctionName
{
    private readonly IDataverseService _dataverse;
    private readonly ILogger<FunctionName> _logger;

    public FunctionName(IDataverseService dataverse, ILogger<FunctionName> logger)
    {
        _dataverse = dataverse;
        _logger = logger;
    }

    [Function(nameof(FunctionName))]
    public async Task<HttpResponseData> Run(
        [HttpTrigger(AuthorizationLevel.Function, "post")] HttpRequestData req)
    {
        _logger.LogInformation("Processing request...");

        // 1. Validate input
        // 2. Authenticate to Dataverse (Managed Identity preferred)
        // 3. Execute business logic
        // 4. Return response

        var response = req.CreateResponse(HttpStatusCode.OK);
        return response;
    }
}
```

**Autenticação Dataverse (ordem de preferência)**:
1. **Managed Identity** (recomendado para Azure-hosted)
2. **App Registration** (client_id + client_secret via Key Vault)
3. **Connection String** (apenas dev local)

---

### Step 3: Service Bus Integration (se aplicável)

**Patterns**:

**Publisher (Plugin → Service Bus)**:
```csharp
// No plugin Post-Operation:
var endpoint = serviceProvider.GetService(typeof(IServiceEndpointNotificationService))
    as IServiceEndpointNotificationService;
endpoint.Execute(serviceEndpointRef, context.PluginExecutionContext);
```

**Subscriber (Azure Function)**:
```csharp
[Function("ProcessMessage")]
public async Task Run(
    [ServiceBusTrigger("topicname", "subscriptionname")] ServiceBusReceivedMessage message)
{
    var body = message.Body.ToString();
    // Process message
    // Update Dataverse
}
```

**Error handling**:
- Max delivery count: 10
- Dead-letter queue monitoring
- Application Insights alerting

---

### Step 4: Webhook Setup (se aplicável)

**Dataverse Webhook → Azure Function**:

```yaml
webhook_registration:
  name: "FTD.[Entity].[Message]Webhook"
  endpoint: "https://ftd-functions.azurewebsites.net/api/[endpoint]"
  authentication:
    type: "HttpHeader"  # ou WebhookKey, HttpQueryString
    key: "x-functions-key"
    value: "[Azure Function key via Key Vault]"
  message: "[Create/Update/Delete]"
  entity: "[logical_name]"
  filtering_attributes: ["attr1", "attr2"]
```

---

### Step 5: Testing Integration

**Níveis de teste**:

1. **Unit Tests** (xUnit + Moq):
   - Mock IDataverseService
   - Mock ServiceBusClient
   - Test business logic isolation

2. **Integration Tests** (ambiente de Test):
   - Real Dataverse connection
   - Real Service Bus
   - End-to-end message flow

3. **Contract Tests**:
   - Validate message schemas
   - Validate API contracts

---

### Step 6: Quality Gate

**Checklist Integration**:
- [ ] ADR de pattern documentado
- [ ] Autenticação via Managed Identity ou App Reg (nunca hardcoded)
- [ ] Secrets em Azure Key Vault (nunca em appsettings.json)
- [ ] Retry policy configurada
- [ ] Dead-letter queue configurada (se Service Bus)
- [ ] Idempotência garantida (correlation ID)
- [ ] Application Insights configurado
- [ ] Unit tests ≥ 80% coverage
- [ ] Integration tests escritos
- [ ] Throttling Dataverse API respeitado (6000 req/5min)
- [ ] Error handling com logging estruturado
- [ ] Documentação de deployment atualizada

**Handoff**: → Carla QA (Integration Test Review)
