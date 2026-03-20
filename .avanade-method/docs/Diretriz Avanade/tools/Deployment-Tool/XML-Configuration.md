# XML Configuration

This chapter explains how the tool can be configured for streamlined deployment when running the .exe from the command-line.

The [configuration examples](https://dev.azure.com/innersource/DSS-Framework/_git/DeploymentTool?path=/DeploymentTool.Console/Xml/Examples&version=GBmaster) can be used as entry points for your own deployment configurations.

> **Tip:** In order to simplify the creation of configuration XML data, you can use the XSD (DeploymentConfiguration.xsd) to apply intellisense on the fly when typing the XML configuration. This is supported out of the box when opening the Avacado Add-On solution in Visual Studio.

## Main Command-Line Syntax
Execute the tool by providing only the respective configuration XML as the only mandatory parameter.

**Example**

`dpa.exe Examples\VariousExamples.xml`

## Parameters
Each configuration XML can have a set of global parameters which can be defined in the following ways:

* **Static:**
 
    `<Parameter name="TestName" value="My Test" />`

* **Piped**, i.e. a parameter can be overwritten via the command-line:

    `<Parameter name="Orgname" value="environment" />`

    can be overwritten from the command-line via

    `-Orgname:myorgname`

* **Dynamic**, i.e. one parameter can be dynamically replaced with another (sequentially):

    `<Parameter name="Url" value="https://{Orgname}.crm4.dynamics.com/" />`

* **Results**, i.e. the results of a command can be stored in a parameter:

    `<FieldToParameterMapping fieldname="version" parametername="Version" />`

* Parameters can also be set within CommandGroups by using the `<SetParameter>` command.

All global parameters can be used in Enable Rules.

All global parameters can also be used in command parameters providing the parameter name in curly brackets, for example to define the connection string:

`<ConnectionString>{ConnectionString}</ConnectionString>`

* **Dynamic Expressions**
can manipulate parameters using [['{Parameter}'.YourExpression(...)]] syntax, e.g. to apply string manipulation rules like Replace or Substring.

    `<Parameter name="TransferFolder" value="C:\Dataverse\Deployment\Transfer\2018-12-13 - Release-ABC" />`

    `<Parameter name="DateOfDeploymentPackageFolder" value="[['{TransferFolder}'.Substring(27,10)]]" />`

    `DateOfDeploymentPackageFolder: 2018-12-13`

## Command Groups
Command Groups can be used to organize Commands which logically belong togther, for example the initialization of a deployment can happen in a dedicated Command Group.

## Enable Rules
Enable Rules can be used to dynamically determine if a Command Group or Command shall be run. This can be used to make deployment configurations context-sensitive and more flexible.

## Connection String
Standard Dynamics / Dataverse connection strings can be used.
They can be defined in `<Parameter>` nodes:

`<Parameter name="ConnectionString" value="AuthType=Office365; Url={Url}; Username={Username}; Password={Password}; " />`
    
And then those parameters can be used in the corresponding `<CrmConnection>`nodes of any Dataverse command.

The connection strings are generically processed by the XRM Tooling (CrmServiceClient class) unless the connection string contains the key value pair AuthType=OAuth because then a specific logic is used to authenticate to Dataverse via OAuth including the provided ClientId and RedirectUri. Tokens are stored in the location provided in TokenCacheStorePath.

> **Tip:** To use multiple Dataverse Connection Strings in the same configuration, the flag `RequireNewInstance=true` must be set. This may be required for example to run `ExportSolution` from Org 1 and `ImportSolution` for Org 2 in the same configuration.
> (See also [CrmTipOfTheDay-798](https://crmtipoftheday.com/798/crmserviceclient-and-multiple-instances/))
>
> **Example:** `"RequireNewInstance=True; Url=https://barbaz.crm.dynamics.com; AuthType=Office365; UserName=user@barbaz.onmicrosoft.com; Password=password";`
