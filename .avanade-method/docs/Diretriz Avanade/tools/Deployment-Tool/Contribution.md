# Contribution

This chapter refers to the extension of the tool by adding new functionalities, mainly by adding new commands.

## Create New Command
This section describes how a new command can be added to the Deployment Tool making use of the existing classes and extensible methods to make sure that the developer can concentrate on the logical extensions.
> **Note:** In the following, the example command name “ExampleCommand” is used.

1. **Open Solution**

    `DeploymentTool.sln`

2. **Add Command to CommandGroup.cs**

    * Open `CommandGroup.cs`
    * Add serialization information `XmlArrayItem(Type = typeof(ApplySystemSetting), IsNullable = true)` in the right location (alphabetical) above the `Commands` array declaration.

> **Note:** Both #2 and #3 allow the correct XML Schema creation for the command so that it can be used in the XML deployment configuration (with intellisense).

3. **Add New Command Class in .cs File**

    * Depending on the command type, you have to choose the .cs file. For example, Dataverse commands go into `DataverseCommands.cs`.
    * Add the code block for the command in the right location (alphabetical).
    * A Command class only requires you to add the input parameters to be used for the command. Use the following sample code:
```
#region
[Serializable]
public class ExampleCommand : DataverseCommand
{
    public string Input { get; set; };
    ...
}
#endregion
```

4. **Add New Controller Class**

    * Analogous to the command, the controller must be added to the right subfolder in `BusinessLayer\Controllers`.

    * You should copy an existing controller to speed up the development.

    * You basically need to consider three things:
      * Constructor (single line): Takes the Command from #3 as the input parameter and passes it to the base constructor.
      * Command property (single line): Returns the Command converted to the right type for easy use in the processing logic.
      * Process() function: This function handles the full logic, make sure to put all CRUD operations into the repository if it does not yet exist there. The following properties will help you:

        | Property | Description |
        | --- | --- |
        | Logger | This is the logging repository (NLog) which can be used in all commands |

5. **Add Tests**

    * Add new class `<Command Name>Test.cs` in respective folder (`Avanade.DPA.Deployment.Tests\<DataverseCommands|OtherCommands|...>) which inherits from appropriate base test class.
    * Utilize base methods `InitializeDeploymentConfiguration()` to setup the deployment configuration for the test.
    * For Dataverse tests, utilize `InitializeCrmOrganizationService()` to set up the faked Dataverse context/service.
    * Add enough test methods to achive 100% code coverage of the command.

6. **Add Example XML File in `Xml\Examples` Folder**

7. **Modify this `README.md` File in "Latest Updates" Section**

## Scribe Command Development
The Scribe Online API does not provide an exhaustive schema for objects retrieved via the API. This means that there could be occasions where a certain property is used for the first time and it would be missed during the serialization in the respective HTTP GET request. This is prominently the case for Advanced Maps and if this happens, the underlying class ScribeAdvancedMap.cs needs to be extended in the following way.

1. Set up a command `<ExportScribeMaps>` referring to the map which has a property not yet contained in `ScribeAdvancedMap.cs`.
2. Set a debugger breakpoint in ExportScribeMapsController.cs for example at `GetMapLinkDetails()`.
3. Debug into the execution chain using F11 until you reach
`if (response.IsSuccessStatusCode)`.
4. Copy the text of content.
5. Go to `ScribeAdvancedMap.cs`.
6. Paste the content as JSON via `Edit -> Paste Special`.
7. Check every property and subclass of the pasted Rootobject class if it is contained in the existing classes. If not, merge this property into the existing `ScribeAdvancedMap` class.
8. Remove the remaining pasted content.
9. Next time you export the map all properties should be contained.