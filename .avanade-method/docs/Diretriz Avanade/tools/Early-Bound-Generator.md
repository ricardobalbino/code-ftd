# Early Bound Generator

> Work in Progress! Please check back soon...

The Early Bound Generator is used to generate [Early Bound Entities](https://docs.microsoft.com/en-us/dynamics365/customer-engagement/developer/org-service/use-early-bound-entity-classes-code) for the Backend (.NET / C#). Checkout the [Early Bound Entity Classes](../development/Backend/Early-Bound-Entity-Classes.md) for information about the usage in the **BizApps Core Accelerator**.

Microsoft provides a tool called [CrmSvcUtil](https://docs.microsoft.com/en-us/dynamics365/customer-engagement/developer/org-service/create-early-bound-entity-classes-code-generation-tool), which generates the entities. This tool can easily be expanded by providing extensions. This is exactly what was done here!

The main advantages of using the Avanade Early Bound Generator Extensions are:
- All Entities are located in their own file (e.g. `Account.cs` and `Contact.cs`)
- Better Naming support
  - Remove Prefixes
  - CamelCase support
- Filter which entities shall be generated (based on Regex expressions)
- Exclude specific Attributes
- Add custom base classes to the generated Entities
- Add custom class attributes (e.g. "ExcludeFromCodeCoverageAttribute")

## Executing the EarlyBoundGenerator

If you are using the **BizApps Core Accelerator** please refer to the [Early Bound Entity Classes](../development/Backend/Early-Bound-Entity-Classes.md) Section. 



## Configuration file

The configuration file contains all information used while entity generation. The following options can be configured:

| Name                                             | Description                                                                                             |
| ------------------------------------------------ | ------------------------------------------------------------------------------------------------------- |
| appSettings -> namespace                         | The namespace of the generated entities                                                                 |
| XrmSettings                                      | Includes all settings specific to the Avanade Early Bound Generator                                     |
| -> CodeGeneration                                | Defines which additional files shall be generated                                                       |
| -> Entities -> Filter                            | A list of regex pattern that specify which entities shall be generated                                  |
| -> Entities -> BaseType                          | A list of (full qualified) classes or interfaces which will be implemented/derived by each entity class |
| -> OptionSets -> generateAllOptionSetType        | True if all option set types shall be generated. Otherwise false                                        |
| -> Attributes -> statusCodeAndStateCodeCamelCase | When true all StateCode and StatusCode Enums will be in CamelCase                                       |
| -> Attributes -> ToExclude                       | List of regex pattern that define which attributes to exclude                                           |
| -> Naming -> uppercaseFirstChar                  | When true the first char of each name will be capitalized                                               |
| -> Naming -> camelCase                           | Gets the camel case name of the option sets                                                             |
| -> Naming -> PrefixesToRemove                    | A List of strings that will be removed from the beginning of each name                                  |
| -> Output -> splitFiles                          | When true each entity will be placed in it's own file                                                   |
| -> Output -> outputDirectory                     | The output directory or the generated entitiy (only when `splitFiles` is enabled)                       |
| -> CodeCustomizing -> LeadingCode                | A list of code parts that will be added to the top of each generated file                               |
| -> CodeCustomizing -> TrailingCode               | A list of code lines that will be added to the bottom of each generated file                            |
| -> CodeCustomizing -> ClassAttributes            | A list of attributes that will be added to each generated file                                          |

An example configuration can be found [HERE](https://innersource.visualstudio.com/DSS-Framework/BizApps Core Accelerator%20Team/_git/Template?path=%2FProject%2FBackend%2FCommon%2FAvanade.Project.Xrm%2FEntities.Generated%2FCrmSvcUtil.exe.config&version=GBmaster)