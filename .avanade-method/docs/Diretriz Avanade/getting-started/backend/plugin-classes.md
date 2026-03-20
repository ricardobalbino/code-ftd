# Plugin Classes

Creating plugins feels quite similar to the regular Microsoft SDK approach. This guide will give you a quick introduction to plugins by using the examples provided with the **BizApps Core Accelerator**.

## How do Plugins Look Like

With the Microsoft SDK you create a plugin by deriving from the `IPlugin` base class and overwriting the `Execute` method. 
The **BizApps Core Accelerator** introduces an additional base class called `Plugin` as well as a generic `Plugin<T>` overload that simplify plugin development.

=== "Microsoft SDK"
    ```csharp
    public class ExamplePlugin : IPlugin
    {
        public void Execute(IServiceProvider serviceProvider)
        {
            throw new NotImplementedException();
        }
    }
    ```

=== "BizApps Core Accelerator (Generic)"
    ```csharp
    public class ExamplePlugin : Plugin<Account>, IPlugin
    {
        public override void Execute(IDependencyContainer container, Account account)
        {
            throw new NotImplementedException();
        }
    }
    ```

=== "BizApps Core Accelerator (Non-Generic)"
    ```csharp
    public class ExamplePlugin : Plugin, IPlugin
    {
        public override void Execute(IDependencyContainer container)
        {
            throw new NotImplementedException();
        }
    }
    ```

When a target is available in e.g. an `Create` or `Update` message, the generic `Plugin<T>` class should be used. This will automatically get the target, in the example above the `Account`, and provides it to you in the `Execute` method. The non-generic `Plugin` class should only be used with special messages, like `Delete` or `RetrieveMultiple` where no target is available.

## How to Register Plugins

Instead of registering the plugins manually through the Plugin Registration Tool provided by Microsoft, the **BizApps Core Accelerator** includes a tool to automate that process. To feed the required information about the plugin registration to this tool, a custom attribute is used and placed on top of your plugin class. Let's have a look at a few example.

=== "AccountCreatePreOpsPlugin"
    ```csharp
    [PluginRegistration(
        EntityLogicalName = Account.EntityLogicalName,
        MessageName = MessageNames.Create,
        Stage = Stage.PreOperation,
        IncludePreImage = false,
        Mode = SdkMessageProcessingStepMode.Synchronous
    )]
    public class AccountCreatePreOpsPlugin : Plugin<Account>, IPlugin
    {
        public override void Execute(IDependencyContainer container, Account account)
        {
            // ...
        }
    }
    ```
=== "AccountUpdatePlugin"
    ```csharp
    [PluginRegistration(
        EntityLogicalName = Account.EntityLogicalName,
        MessageName = MessageNames.Update,
        Stage = Stage.PostOperation,
        IncludePreImage = true,
        Mode = SdkMessageProcessingStepMode.Asynchronous
    )]
    public class AccountUpdatePlugin : Plugin<Account>, IPlugin
    {
        public override void Execute(IDependencyContainer container, Account account)
        {
            // ...
        }
    }
    ```
=== "AccountRetrieveMultiplePlugin"
    ```csharp
    [PluginRegistration(
        EntityLogicalName = Account.EntityLogicalName,
        MessageName = MessageNames.RetrieveMultiple,
        Stage = Stage.PreValidation,
        IncludePreImage = false,
        Mode = SdkMessageProcessingStepMode.Synchronous
    )]
    public class AccountRetrieveMultiplePlugin : Plugin, IPlugin
    {
        public override void Execute(IDependencyContainer container)
        {
            // ...
        }
    }
    ```

The properties of the `PluginRegistration` attribute should be familiar from the Plugin Registration Tool provided by Microsoft. Refer to the official Microsoft documentation for more details: [Register a plug-in](https://docs.microsoft.com/en-us/powerapps/developer/data-platform/register-plug-in#general-configuration-information-fields){ target=_blank }.

In case you want to register the plugin for multiple steps, you can place multiple `PluginRegistration` attributes above the plugin.

!!! info "Plugin Design Considerations"
    Like the Microsoft SDK, BCA supports any kind of plugin structure and design and provides full flexibility. Nevertheless, the guidance given in this Wiki and the example code applies a shift from logic-specific plugins (e.g. `PrefillCaseWhenStatusInProgressPlugin`) to registration-specific plugins (e.g. `<Table><Stage><Message><Mode>Plugin`) for the following benefits:

    * __Performance__: Microsoft resolves plugins in a way which adds overhead to each plugin class execution, so by reducing the number of plugins and executing the logic inside the registration-specific plugins avoids this overhead.
    * __Transparency__: You are always see what exactly is registered and when.
    * __Flexibility__: With the managed solution approach and the the way how obsolete plugins are treated in Dynamics (plugin cemetery), this approach is much less stressful and involves less maintenance.
    
    In order to execute multiple logics in a plugin, one just needs to resolve the services individually and then call them one by one.
    
    You can also simplify the statements directly and make individual statements directly, for example:
    
    ```csharp
    Container.Resolve<ITheInterface1>().Execute(...)
    Container.Resolve<ITheInterface2>().DoWhatever(...)
    ```
 
## Plugins as the Anti Corruption Layer

**What is an Anti Corruption Layer?**

> Implement a façade or adapter layer between different subsystems that don't share the same semantics. This layer translates requests that one subsystem makes to the other subsystem. Use this pattern to ensure that an application's design is not limited by dependencies on outside subsystems. -- From [Anti-corruption Layer pattern, Microsoft](https://docs.microsoft.com/en-us/azure/architecture/patterns/anti-corruption-layer)

In case of the **BizApps Core Accelerator**, the business logic is not strictly dependent on Dataverse / Dataverse and can easily be reused inside e.g. an Azure Function or a Console Line Application. The only part of your project that is dependent on the Dataverse Plugin infrastructure is the Plugin class itself. This means, it acts as the Anti Corruption Layer.

In more practical terms this means that the plugin should not contain any business logic but instead just call a service. Let's have a look at a few examples `Execute` method:

=== "AccountCreatePreOpsPlugin"
    ```csharp
    [PluginRegistration(
        EntityLogicalName = Account.EntityLogicalName,
        MessageName = MessageNames.Create,
        Stage = Stage.PreOperation,
        IncludePreImage = false,
        Mode = SdkMessageProcessingStepMode.Synchronous
    )]
    public class AccountCreatePreOpsPlugin : Plugin<Account>, IPlugin
    {
        public override void Execute(IDependencyContainer container, Account account)
        {
            var prefillService = container.Resolve<IAccountPrefillService>();
            prefillService.PrefillAccount(account);
        }
    }
    ```
=== "AccountUpdatePlugin"
    ```csharp
    [PluginRegistration(
        EntityLogicalName = Account.EntityLogicalName,
        MessageName = MessageNames.Update,
        Stage = Stage.PostOperation,
        IncludePreImage = true,
        Mode = SdkMessageProcessingStepMode.Asynchronous
    )]
    public class AccountUpdatePlugin : Plugin<Account>, IPlugin
    {
        public override void Execute(IDependencyContainer container, Account account)
        {
            var syncService = container.Resolve<IAccountContactSyncService>();
            syncService.SyncAccountDetailsToPrimaryContact(account);
        }
    }
    ```
=== "AccountDeletePlugin"
    ```csharp
    [PluginRegistration(
        EntityLogicalName = Account.EntityLogicalName,
        MessageName = MessageNames.Delete,
        Stage = Stage.PreOperation,
        IncludePreImage = false,
        Mode = SdkMessageProcessingStepMode.Synchronous
    )]
    public class AccountDeletePlugin : Plugin
    {
        public override void Execute(IDependencyContainer container)
        {
            var executionContext = container.Resolve<IPluginExecutionContext>();
            var deleteRequest = new DeleteRequest { Parameters = executionContext.InputParameters };

            var cleanUpService = container.Resolve<IAccountCleanupService>();
            cleanUpService.CleanUp(deleteRequest.Target.Id);
        }
    }
    ```

All examples above only have 2-4 lines of code in their `Execute` method, and that is all that's needed! It acts as the façade for the individual service (`IAccountPrefillService`, `IAccountContactSyncService` and `IAccountCleanupService`). Before we have a look at the services and see how they work, we have to look at one essential concept before: The Dependency Container. In the examples above you can already see the `IDependencyContainer` in action and how it is used to retrieve some services. Head over into the next page to understand what this is all about.