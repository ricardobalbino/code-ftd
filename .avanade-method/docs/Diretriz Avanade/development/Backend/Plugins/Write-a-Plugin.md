# Write a Plugin

## Structure
All plugins do look somewhat similar to Standard CrmSDK style plugins.

A BizApps Core Accelerator plugin looks like this:
```csharp
public class AccountCreatePostOpsPlugin : Plugin<Account>
{
    public override void Execute(IDependencyContainer container, Account account)
    {
        ...
    }
}
```

Instead of deriving from IPlugin the Accelerator provides a base class `Plugin<>` which takes care of the [IoC container](../Dependency-Injection.md) which will be passed into the `Execute` method as the first parameter.

> The BizApps Core Accelerator provides a Visual Studio extension which ease the process of creating new plugins. Please head on over [here](../../Visual-Studio-Extension.md) for more details.

## Anti Corruption - Guarding the business logic
Using the IoC container we're able to "protect" the business logic from dealing with any Dataverse specifics and CrmSDK classes thus making it independent from the ecosystem where it is being executed. This enables the deployment of the same business logic outside of a plugin e.g. as an Azure Function, web worker role, Command Line Tool, Windows Service, etc.

The plugin should solely contain high level logic. It acts as a dispatcher towards any implemented [business logic services](./Services-aka-Business-Logic.md).

## Plugins for "special" messages

Using the above generic plugin class won't work for special messages (e.g. `WinOpportunity`, `Delete`, `RetrieveMultiple`) since the Target parameter isn't of type Entity. To remove dependencies on the Accelerator to support special messages you can simply use a non generic `Plugin` base class.

For a delete message your plugin would look like this:
```csharp
public class AccountDeletePreOperation : Plugin
{
    public override void Execute(IDependencyContainer container)
    {
        var context = container.Resolve<IPluginExecutionContext>();
        var entityReference = context.InputParameters[InputParameters.Target] as EntityReference;
        
        var deletionService = container.Resolve<IHandleAccountDeletions>();
        deletionService.Process(entityReference.Id);
    }
}
```

Since our plugins are our anti corruption layer we're dealing with the unboxing of certain special objects/messages in them. Only providing abstracted information down to our business logic layer. 

Using the non generic `Plugin` base class you can deal with any special message on which you're registering your plugins.
Please check the example plugins in your repository to additional example code.

### The target

Every plugin gets the target passed into the `Execute` method as the second parameter. The parameter relates to the target inside of the `InputParemeters` provided by the `PluginExecutionContext`. Thus when working with the target you're always working with the target provided by the `InputParameters`.

For more details look into on how we do [Target handling](./Handling-the-target.md).

Additionally the target contains the filtered attributes which are being registered using the Automated Plugin Registration Tool + the changed attributes. This is being done automatically by utilizing the Workspace Analyzer which analysis the project solution and determines the actual used attributes (Early- & Late-Bound). Please navigate to the Workspace Analyzer for further information on how the analysis of the project solution is being integrated into the whole CI/CD pipeline.

This automated filtered attributes analysis can be overwritten. For further details please read [Register plugin](Register-Plugins.md)

## Putting plugins and Services together

Using the concept of dependency inversion, described [here](../Dependency-Injection.md), we are able to use any [implemented service](Services-aka-Business-Logic.md) into any plugin.

A plugin using the `IAccountPrefillService` looks the following:

```csharp
public class AccountCreatePostOpsPlugin : Plugin<Account>
{
    public override void DoExecute(IDependencyContainer container, Account account)
    {
        var prefiller = container.Resolve<IAccountPrefillService>();
        prefiller.Prefill(account);
    }
}
```

With the high level requirements described in the `Plugin` and the low level details/domain logic implemented in a `Service` this code provides enough readability for any colleague on the project. The code can even be used as some sort of documentation (what happens when).