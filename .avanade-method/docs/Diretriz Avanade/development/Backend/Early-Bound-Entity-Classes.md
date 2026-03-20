# Early Bound Entities (Backend)

For the .NET / C# backend code we generate [Early Bound Entities](https://docs.microsoft.com/en-us/dynamics365/customer-engagement/developer/org-service/create-early-bound-entity-classes-code-generation-tool). Usig early bound entites has numerous advantages:

- Less, more readable code
- Better IntelliSense support
- Type safety
- You can verify entity, attribute, and relationship names at compile time

Microsoft itself offers a tool called `CrmSvcUtil.exe` which handles the entity generation. It has been extended to generate additional classes and to handle more use-cases.

The **Early Bound Entities** are located at `Project\Backend\Common\Dataverse.Common\Entities.Generated`. That's also the location of the `GenerateEarlyBoundEntities` PowerShell script which handles the generation. It utilizes the Configuration.json to get the Dataverse connection details for the current branch.

The file `CrmSvcUtil.exe.config` contains the configuration. It defines which entities to generate and has a variety of customizing options. More details about the configuration can be found [HERE](../../tools/Early-Bound-Generator.md).

## Usage

To get started generating the **Early Bound Entities** you need to edit the configuration file `CrmSvcUtil.exe.config` which is located at `Project\Backend\Common\Dataverse.Common\Entities.Generated`. Add all entities you want to include in the generation process to the `XrmSettings->Entities->Filter` section of the XML document. 

Save the file, execute the either the `GenerateEarlyBoundEntities.ps1` powershell script or execute the CLI (`GenerateEarlyBound`) and wait for it to finish.

## Interactive Login

In some scenarios you can't use the simple user/password authentication mechanism. In these circumstances you can enable the interactive login (OAuth) of the CrmSvcUtil. To do this please open the PowerShell script "GenerateEarlyBoundEntities.ps1" at `Project\Backend\Common\Xrm\Entities.Generated` and remove the "#" from the following line:

```ps
...
# $command += " /il" // Enables interactive login
```

When calling the PowerShell script the next time the Dataverse login window will pop up.   