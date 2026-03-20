# Register Plugins

Typically you would register plugins using the Plugin Registration Tool provided by Microsoft. Every class which is derived from `IPlugin` is a PluginType for which you can add plugin steps.

The BizApps Core Accelerator provides an automated plugin registration mechanism which you can leverage by simply adding/maintaining a specific class attribute. This ensures that the code is the truth and the developer is not forced to leave his "zone" to do deployment tasks.

## Plugin attributes

The Automated Plugin Registration Tool (APRT) uses [reflection](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/reflection) to analyze the DLL. 

Using the provided parameters the APRT searches for classes which are decorated with the PluginRegistration attribute. You can see an example of such a decorated class below:

```csharp
    [PluginRegistration(
        EntityLogicalName = Account.EntityLogicalName,
        MessageName = MessageNames.Create,
        Stage = Stage.PostOperation,
        IncludePreImage = false,
        AsAsync = false
    )]
    public class AccountCreatePostOpslugin : Plugin<Account>
    {
        public override void DoExecute(IDependencyContainer container, Account account)
        {
            ...
        }
    }
```

The attribute provides the following configurable properties:

| Name                | Purpose                                                                        | Values                                                          |
|---------------------|--------------------------------------------------------------------------------|-----------------------------------------------------------------|
| EntityLogicalName   | The Entity for which the plugin step shall be triggered                        | Logical name of the entity                                      |
| Stage               | The Stage for which the plugin step shall be triggered                         | PreValidation, PreOperation, PostOperation                      |
| MessageName         | The message for which the plugin step shall be triggered                       | Create, Update, Delete, ...                                     |
| IncludePreImage     | Should the plugin step include a PreImage                                      | True, False                                                     |
| AsAsync             | Should the plugin step be executed as asynchronously                           | True,False                                                      |
| ExecutionOrder      | Order in which the given plugin step should be executed                        | Numeric                                                         |
| FilteringAttributes | List of attributes for when the plugin should be triggered                     | All valid attributes of the respective entity (EntityNames.X.Y) |
| PreImageAttributes  | List of attributes the PreImage should contain                                 | All valid attributes of the respective entity (EntityNames.X.Y) |
| AsAdmin             | Registers the plugin as CurrentUser but provides an admin dependency container | True,False                                                      |

> **Registering a plugin with AsAdmin = true should be the last option simply because the whole pipeline gets executed as the SYSTEM user. Please refer to [this](../Execute-As-Admin.md) page to validate other options.**

## Deploying a plugin

After adjusting the PluginRegistration attributes' properties simply create a new commit, push the changes to the remote branch and queue a new "Deploy Code" pipeline. 