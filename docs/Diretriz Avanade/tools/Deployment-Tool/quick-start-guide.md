# Quick Start Guide

## First Install

### Option 1

When you have the BizApps Core Accelerator set up locally, just run the `bizapps-cli.ps1` and select the `InstallTools` action.

### Option 2

In order to get the Deployment Tool installed on your machine standalone, you can use the `bizapps-cli.ps1` or the [InstallTools.ps1](https://dev.azure.com/innersource/DSS-Framework/_git/Template?path=%2Fcli%2FInstallTools.ps1) script directly.

### Option 3

Alternatively, get the latest tool binaries from here:

[![Avanade.BizApps.Tools.DeploymentTool package in DSS feed in Azure Artifacts](https://feeds.dev.azure.com/innersource/_apis/public/Packaging/Feeds/99c7c714-98e8-4403-9530-78a81a707edf/Packages/ff914f59-2107-45ba-823f-3b47a217622d/Badge)](https://dev.azure.com/innersource/DSS-Framework/_packaging?_a=package&feed=99c7c714-98e8-4403-9530-78a81a707edf&package=ff914f59-2107-45ba-823f-3b47a217622d&preferRelease=true)

## Approach A: Command-Line

1. Take any of the [example XML configurations](https://dev.azure.com/innersource/DSS-Framework/_git/DeploymentTool?path=/DeploymentTool.Console/Xml/Examples).

2. Make adjustments according to your environment.

3. Open the command-line and run the tool against the configuration XML, for example:
`dpa.exe Examples\MyExample.xml`

4. Optionally pipe or override parameters by adding switches like `-Orgname:"orgname"` to the command-line call.

[List of Commands](https://dev.azure.com/innersource/DSS-Framework/_git/DeploymentTool?path=/DeploymentTool.Doc/Commands.md&version=GBmaster&_a=preview)

## Approach B: PowerShell Cmdlets

1. Import the module via `Import-Module <Path where DPA has been downloaded to>\Avanade.BizApps.Tools.DeploymentTool.Cmdlets.dll`

2. Run `Connect-Dataverse -ConnectionString <Your connection string>`.

3. Execute Cmdlets such as `Get-DataverseVersion`.

For a list of all available Cmdlets and further information, see [here](./PowerShell-Cmdlets.md).

!!! tip "Pro-Tip"
    If you are using `Windows Terminal`, you can easily set up a default PowerShell session which is preconfigured to connect to your environment of choice to improve productivity:

    1. Open `Windows Terminal`.
    1. Go to `Settings`.
    1. Add new profile.
    1. Add the following in the command-line (provide the connection string as by your preference): 
       ```pwsh
       powershell.exe  -NoExit -Command "$InformationPreference = 'Continue';Import-Module <Deployment Tool Location>\Avanade.BizApps.Tools.DeploymentTool.Cmdlets.dll; $connectionString='Url=https://<Your Dataverse Organization>.crm4.dynamics.com/; AuthType=ClientSecret; ClientId=<Client ID>; ClientSecret=<Client Secret>'; Connect-Dataverse -ConnectionString $connectionString;"
       ```
    1. Click `Save`.

    When you start the start the corresponding session from your terminal, you are immediately connected and can run commands against your environment. 
