# Automate Dataverse Tasks with Cmdlets

When workin with the BCA, deployment automation is part of the regular pipeline and should be handled as described [here](../../../getting-started/next-steps/deployment-automation.md). However, as soon as you are dealing any task automation outside the regular pipeline, you can use the DPA to either run the command-line with an [XML configuration](../XML-Configuration.md) or automate the task via a PowerShell script using the [DPA Cmdlets](../PowerShell-Cmdlets.md).

This tutorial is focused on the latter usage of Cmdlets and provides an example how you can automate the export of one or several solutions which can be helpful for backup purposes or when refactoring solutions and moving them into the final customization master environment.

## 1. Set Up DPA for Cmdlet Usage

Before using the DPA Cmdlets, you need to prepare your local environment which can be done in different ways, for example by following the pro-tip to use [Windows Terminal](../quick-start-guide.md#approach-b-powershell-cmdlets).

At the very least, you need to run the tool install or a NuGet restore of the BCA project which will store the DPA in your local machine folder, typically `C:\Users\<your user id>\.nuget\packages\avanade.bizapps.tools.deploymenttool\<current version>`.

To import the Cmdlet, run the following line in Windows Powershell:

```pwsh
Import-Module C:\Users\<your user id>\.nuget\packages\avanade.bizapps.tools.deploymenttool\<current version>\content\Avanade.BizApps.Tools.DeploymentTool.Cmdlets.dll
```

!!! warning "PowerShell Version"
    Make sure that you are using `Windows PowerShell` and not `PowerShell 7.x` (PowerShell Core) because Microsoft Dataverse requires the Windows version.

## 2. Dataverse Connection

Connect to your Dataverse instance using the `Connect-Dataverse` Cmdlet. You can directly provide the connection string or tap into the `Configuration.json` of your BCA project and use the connection string of any environment by running the following code:

```pwsh
$environment = "Dev"
$configuration = Get-Content C:\VS\MyProject\src\Configuration.json -Raw | ConvertFrom-Json
$connectionString = $configuration.Environments."$environment".ConnectionString
Connect-Dataverse -ConnectionString $connectionString
```

!!! tip
    If you want to reconnect to a different environment inbetween your task automation, just run `Connect-Dataverse` with a connection string pointing to the other environment.

## 3. Cmdlet Orchestration

By running `Get-Command -Module Avanade.BizApps.Tools.DeploymentTool.Cmdlets`, you can list all available DPA Cmdlets. 

Many of the Cmdlets return objects which you can use in the further processing. For example, you can run `$solutions = (Get-Solutions).uniquename | Sort-Object` to store a sorted list of the unique names of all solutions in the variable `$solutions`. You can use this Cmdlet also to check if a solution exists:

```pwsh
if ($null -eq ((Get-Solutions) | Where-Object { $_.uniquename -eq $SolutionName })) { 
    throw "Solution with uniquename $SolutionName does not exist."
}
```

In order to export a solution, run:

```pwsh
Export-Solution -SolutionName $SolutionName -SolutionExportFile $SolutionName.zip
```

!!! tip
    If you do not want the return object to be printed to the command-line, use `<Cmdlet> | Out-Null`. 

## 4. Final Code

The full code of the export solution example with helpful surrounding logic is as follows. Save the script for example under `automate-dataverse.ps1`:

```pwsh
<#
    .SYNOPSIS
    Solution export via DPA Cmdlets.

    .DESCRIPTION
    This script is used to export solutions from one environment to demonstrate task automation with the DPA Cmdlets. 
#>

[CmdletBinding()]
param(
    [Parameter(Mandatory, HelpMessage = "Location of Avanade.BizApps.Tools.DeploymentTool.Cmdlets.dll, e.g. 'C:\Users\armin.a.fischer\.nuget\packages\avanade.bizapps.tools.deploymenttool\3.0.8\content\Avanade.BizApps.Tools.DeploymentTool.Cmdlets.dll'")][string]$DpaCmdletsLocation,
    [Parameter(Mandatory, HelpMessage = "Location of Configuration.json, e.g. 'C:\VS\DSS\Template\src\Configuration.json'.")][string]$ConfigurationJsonLocation,
    [Parameter(Mandatory, HelpMessage = "Name of the environment configured in Configuration.json, e.g. 'CustomizationMaster'.")][string]$Environment,
    [Parameter(Mandatory, HelpMessage = "Unique name of the solution to be exported.")][string]$SolutionName,
    [Parameter(HelpMessage = "Location where exported solution shall be stored.")][string]$ExportFolder
)

# Load DPA Cmdlets
Import-Module $DpaCmdletsLocation

# Get and parse the BCA Configuration.json
$configuration = Get-Content $ConfigurationJsonLocation -Raw | ConvertFrom-Json

# Retrieve connection string from Configuration.json for the given environment
$connectionString = $configuration.Environments."$environment".ConnectionString

# Connect to Dataverse
Connect-Dataverse -ConnectionString $connectionString | Out-Null

# Check if solution exists
if ($null -eq ((Get-Solutions) | Where-Object { $_.uniquename -eq $SolutionName })) { 
    throw "Solution with uniquename $SolutionName does not exist."
}

# Export solution
$dateFormatted = Get-Date -format "yyyyMMddHHmmss"
Export-Solution -SolutionName $SolutionName -SolutionExportFile (Join-Path -Path $ExportFolder -ChildPath "$($SolutionName)_$dateFormatted.zip")
```

## 5. Script Execution

Run the script manually or via Task Scheduler or similar:

```pwsh
.\automate-dataverse.ps1 -DpaCmdletsLocation C:\Users\armin.a.fischer\.nuget\packages\avanade.bizapps.tools.deploymenttool\3.0.8\content\Avanade.BizApps.Tools.DeploymentTool.Cmdlets.dll -ConfigurationJsonLocation C:\VS\DSS\Template\src\Configuration.json -Environment Dev -SolutionName AvanadeCoreComponentsDemo -ExportFolder C:\SolutionExport
```