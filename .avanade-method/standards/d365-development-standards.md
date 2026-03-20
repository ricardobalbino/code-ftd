# ============================================================
# D365 CE Development Standards - Avanade Method v4.29.0
# ============================================================
# Guia de padrões e boas práticas para desenvolvimento D365 CE
# Loaded by: Tiago Dev, Wilson Architect, Carla QA

---

# 📏 Dynamics 365 CE - Development Standards

## 1. Naming Conventions

### 1.1 Dataverse Components

| Component | Convention | Example |
|-----------|-----------|---------|
| Publisher Prefix | `ftd` (lowercase, 3-5 chars) | `ftd` |
| Table (logical) | `ftd_[entityname]` | `ftd_projecttask` |
| Table (display) | PascalCase com espaços | `Project Task` |
| Column (logical) | `ftd_[columnname]` | `ftd_estimatedhours` |
| Column (display) | Title Case | `Estimated Hours` |
| Global Option Set | `ftd_[name]` | `ftd_taskstatus` |
| Relationship | `ftd_[parent]_[child]` | `ftd_project_projecttask` |
| View | `[Entity] - [Purpose]` | `Active Project Tasks` |
| Form | `[Entity] - [Context]` | `Project Task - Main` |
| Business Rule | `[Entity]: [Rule Description]` | `Account: Require Phone when VIP` |
| Dashboard | `[Module] - [Purpose]` | `Sales - Pipeline Overview` |

### 1.2 Code Components

| Component | Convention | Example |
|-----------|-----------|---------|
| Plugin Class | `[Entity][Message][Stage]` | `AccountPreCreate` |
| Plugin Namespace | `FTD.Plugins.[Module]` | `FTD.Plugins.Sales` |
| Plugin Test | `[PluginClass]Tests` | `AccountPreCreateTests` |
| Custom API | `ftd_[ActionName]` | `ftd_CalculateDiscount` |
| Azure Function | `FTD.Functions.[Domain].[Name]` | `FTD.Functions.Integration.SyncAccount` |
| PCF Control | `FTD[ControlName]Control` | `FTDRatingStarsControl` |
| Web Resource | `ftd_/scripts/[module]/[file].js` | `ftd_/scripts/sales/account.form.js` |
| Web Resource (CSS) | `ftd_/styles/[module]/[file].css` | `ftd_/styles/common/theme.css` |
| Web Resource (HTML) | `ftd_/html/[module]/[file].html` | `ftd_/html/sales/dashboard.html` |

### 1.3 Power Automate

| Component | Convention | Example |
|-----------|-----------|---------|
| Cloud Flow | `FTD - [Module] - [Action] - [Trigger]` | `FTD - Sales - NotifyManager - OnOpportunityWon` |
| Environment Variable | `ftd_[VariableName]` | `ftd_ApiBaseUrl` |
| Connection Reference | `ftd_[SystemName]_[Type]` | `ftd_Dataverse_ServiceAccount` |

### 1.4 Solutions

| Component | Convention | Example |
|-----------|-----------|---------|
| Solution Name | `FTD[Module]` | `FTDSales`, `FTDCore`, `FTDPlugins` |
| Solution Display | `FTD - [Module Description]` | `FTD - Sales Customizations` |
| Version | SemVer: `MAJOR.MINOR.BUILD.REVISION` | `1.2.0.0` |

---

## 2. Plugin Development Standards

### 2.1 Project Structure

```
src/
├── FTD.Plugins/
│   ├── FTD.Plugins.csproj           # Target: net462 (D365 requirement)
│   ├── Base/
│   │   ├── PluginBase.cs            # Abstract base with context helpers
│   │   └── Constants.cs             # GUIDs, option set values, etc.
│   ├── Sales/
│   │   ├── OpportunityPreCreate.cs
│   │   └── OpportunityPostUpdate.cs
│   ├── Service/
│   │   └── IncidentPostCreate.cs
│   ├── Helpers/
│   │   ├── DataverseHelper.cs       # Reusable service calls
│   │   └── ValidationHelper.cs
│   └── Models/
│       └── EarlyBound/              # Generated via pac modelbuilder
│           └── Entities.cs
│
├── FTD.Plugins.Tests/
│   ├── FTD.Plugins.Tests.csproj     # FakeXrmEasy.v9 + xUnit
│   ├── Sales/
│   │   ├── OpportunityPreCreateTests.cs
│   │   └── OpportunityPostUpdateTests.cs
│   └── TestHelpers/
│       └── TestDataFactory.cs       # Shared test data builders
│
└── FTD.Functions/
    ├── FTD.Functions.csproj          # Target: net8.0 (isolated)
    ├── Program.cs
    └── ...
```

### 2.2 Plugin Base Class

```csharp
/// <summary>
/// Base class for all FTD plugins.
/// Provides context helpers and standard error handling.
/// </summary>
public abstract class PluginBase : IPlugin
{
    public void Execute(IServiceProvider serviceProvider)
    {
        var context = (IPluginExecutionContext)serviceProvider
            .GetService(typeof(IPluginExecutionContext));
        var serviceFactory = (IOrganizationServiceFactory)serviceProvider
            .GetService(typeof(IOrganizationServiceFactory));
        var tracingService = (ITracingService)serviceProvider
            .GetService(typeof(ITracingService));
        var service = serviceFactory.CreateOrganizationService(context.UserId);

        var localContext = new LocalPluginContext(context, service, tracingService);

        try
        {
            ExecutePlugin(localContext);
        }
        catch (InvalidPluginExecutionException)
        {
            throw; // Re-throw business errors as-is
        }
        catch (Exception ex)
        {
            tracingService.Trace("Unhandled exception: {0}", ex.ToString());
            throw new InvalidPluginExecutionException(
                "An unexpected error occurred. Please contact your administrator.", ex);
        }
    }

    protected abstract void ExecutePlugin(LocalPluginContext context);
}
```

### 2.3 Mandatory Rules

| Rule | Rationale |
|------|-----------|
| **NEVER** use `async/await` or `Thread` | Dataverse sandbox doesn't support it |
| **NEVER** access filesystem or network | Sandboxed execution environment |
| **NEVER** use static state | Plugins are stateless, shared across threads |
| **ALWAYS** use `ITracingService` | Only supported logging mechanism |
| **ALWAYS** check `Depth > 1` | Prevent infinite recursion |
| **ALWAYS** use Early-Bound entities | Type safety, refactoring support |
| **ALWAYS** specify `ColumnSet` columns | Never use `ColumnSet(true)` |
| **NEVER** hardcode GUIDs | Use constants or configuration |
| **ALWAYS** throw `InvalidPluginExecutionException` | Standard D365 business error |

---

## 3. Azure Function Standards

### 3.1 Authentication to Dataverse

**Recommended: Managed Identity**
```csharp
// Program.cs
builder.Services.AddSingleton(sp =>
{
    var credential = new DefaultAzureCredential();
    var client = new ServiceClient(
        new Uri(Environment.GetEnvironmentVariable("DATAVERSE_URL")),
        tokenProviderFunction: async (url) =>
        {
            var token = await credential.GetTokenAsync(
                new TokenRequestContext(new[] { $"{url}/.default" }));
            return token.Token;
        });
    return client;
});
```

### 3.2 Dataverse API Throttling

| Limit | Value | Strategy |
|-------|-------|----------|
| API requests | 6,000 per 5-minute sliding window per user | Batch operations, throttle |
| ExecuteMultiple | Max 1,000 requests per batch | Split larger batches |
| Concurrent connections | 52 per user | Connection pooling |

### 3.3 Retry Policy

```csharp
// Polly retry for Dataverse 429 (Too Many Requests)
builder.Services.AddResiliencePipeline("dataverse", pipeline =>
{
    pipeline.AddRetry(new RetryStrategyOptions
    {
        MaxRetryAttempts = 3,
        Delay = TimeSpan.FromSeconds(2),
        BackoffType = DelayBackoffType.Exponential,
        ShouldHandle = new PredicateBuilder()
            .Handle<FaultException>(ex => ex.Message.Contains("429"))
    });
});
```

---

## 4. PCF Control Standards

### 4.1 Initialization

```bash
# Create new PCF control
pac pcf init --namespace FTD --name [ControlName] --template field --framework react

# Build and test
npm install
npm run build
npm test

# Push to environment (dev only)
pac pcf push --publisher-prefix ftd
```

### 4.2 Lifecycle

```typescript
export class FTDControl implements ComponentFramework.StandardControl<IInputs, IOutputs> {
    private _container: HTMLDivElement;
    private _notifyOutputChanged: () => void;

    init(context: ComponentFramework.Context<IInputs>,
         notifyOutputChanged: () => void,
         state: ComponentFramework.Dictionary,
         container: HTMLDivElement): void {
        this._container = container;
        this._notifyOutputChanged = notifyOutputChanged;
        // Setup React root, event listeners
    }

    updateView(context: ComponentFramework.Context<IInputs>): void {
        // Re-render with updated context
        // ONLY re-render if inputs changed
    }

    getOutputs(): IOutputs {
        // Return bound field values
        return { /* bound values */ };
    }

    destroy(): void {
        // CRITICAL: Clean up React root, event listeners, timers
        // Prevent memory leaks
    }
}
```

---

## 5. Power Automate Standards

### 5.1 Flow Structure (Mandatory)

```
Trigger
├── Scope: Initialize
│   ├── Initialize Variable: varStatus
│   ├── Initialize Variable: varErrorMessage
│   └── (other initializations)
├── Scope: Try
│   ├── (Business Logic)
│   ├── Condition / Switch
│   ├── Dataverse Operations
│   └── Set Variable: varStatus = "Success"
├── Scope: Catch [Configure run after: has failed, has timed out]
│   ├── Compose: Error Details (from outputs of failed actions)
│   ├── Set Variable: varErrorMessage = [error details]
│   └── Send Notification (email/Teams/log)
└── Scope: Finally [Configure run after: is successful, has failed, is skipped, has timed out]
    └── (Cleanup, audit, status update)
```

### 5.2 Mandatory Configurations

| Configuration | Value | Reason |
|--------------|-------|--------|
| Concurrency Control | ON (degree=1 unless parallel needed) | Prevent race conditions |
| Retry Policy | Fixed (count=3, interval=PT30S) | Resilience |
| Run After (Catch) | "has failed", "has timed out" | Error handling |
| Timeout (long actions) | PT5M or appropriate | Prevent hanging |
| Connection Reference | Always use (not direct connection) | ALM portability |

---

## 6. JavaScript Web Resource Standards

### 6.1 Namespace Pattern

```javascript
// CORRECT: Namespace pattern with IIFE
var FTD = window.FTD || {};
FTD.Sales = FTD.Sales || {};

FTD.Sales.Account = (function() {
    "use strict";

    function onLoad(executionContext) {
        var formContext = executionContext.getFormContext();
        // Form initialization logic
    }

    function onPhoneChange(executionContext) {
        var formContext = executionContext.getFormContext();
        var phone = formContext.getAttribute("telephone1").getValue();
        // Validation logic
    }

    return {
        onLoad: onLoad,
        onPhoneChange: onPhoneChange
    };
})();
```

### 6.2 Mandatory Rules

| Rule | Rationale |
|------|-----------|
| Use `formContext` (not `Xrm.Page`) | `Xrm.Page` is deprecated |
| Use `Xrm.WebApi` for CRUD | Standard D365 Web API wrapper |
| Use form notifications (not `alert()`) | UX consistency |
| Always null-check attribute values | Attributes may not exist on form |
| Async via `Xrm.WebApi.[method]().then()` | Promise-based, non-blocking |

---

## 7. Solution Management Standards

### 7.1 Solution Layering

```
[Managed - Microsoft Base]          ← Never touch
    └── [FTDCore - Managed]         ← Foundation: tables, roles, option sets
        ├── [FTDSales - Managed]    ← Sales module customizations
        ├── [FTDService - Managed]  ← Service module customizations
        ├── [FTDPlugins - Managed]  ← Plugin assemblies
        └── [FTDIntegration - Managed] ← Integration components
```

### 7.2 Version Control

| Action | Command |
|--------|---------|
| Export unmanaged | `pac solution export --path ./solutions --name FTDCore --managed false` |
| Unpack solution | `pac solution unpack --zipfile FTDCore.zip --folder ./src/solutions/FTDCore --processCanvasApps` |
| Pack solution | `pac solution pack --zipfile FTDCore_managed.zip --folder ./src/solutions/FTDCore --type Managed` |
| Import managed | `pac solution import --path FTDCore_managed.zip --activate-plugins` |

### 7.3 Branching Strategy

```
main (production)
├── release/v1.x (release stabilization)
├── develop (integration)
│   ├── feature/STORY-001-plugin-validation
│   ├── feature/STORY-002-pcf-rating
│   └── feature/STORY-003-flow-notification
└── hotfix/HOTFIX-001-fix-plugin-error
```

---

## 8. Security Standards

### 8.1 Least Privilege Principle

- Security roles: Mínimo de permissões necessárias
- Field security: PII e dados sensíveis protegidos
- BU structure: Segregação de dados quando necessário
- API permissions: Scope mínimo para App Registrations

### 8.2 LGPD/GDPR Compliance

| Requirement | Implementation |
|------------|---------------|
| Data minimization | Collect only necessary fields |
| Right to access | Audit log + data export capability |
| Right to erasure | Bulk delete + cascade configured |
| Consent tracking | Custom table for consent records |
| Data protection | Field security for PII columns |
| Audit logging | Enable for sensitive tables |

---

## 9. Performance Standards

| Metric | Target | How to Measure |
|--------|--------|----------------|
| Plugin execution | < 2 seconds | ITracingService timestamps |
| Form load time | < 3 seconds | Browser DevTools / Monitor |
| View load time | < 5 seconds (1000 records) | D365 Performance Insights |
| API response | < 1 second (single op) | Application Insights |
| Flow execution | < 5 minutes (standard) | Flow run history |
