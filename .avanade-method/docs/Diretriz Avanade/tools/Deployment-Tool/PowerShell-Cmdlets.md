# PowerShell Cmdlets

You can alternatively use the tool via PowerShell by simply importing the Deployment Tool Cmdlets like any other PowerShell module.

!!! important
    As Dataverse only supports .NET Framework and not .NET Framework Core yet, you need to use Windows PowerShell and not PowerShell Core (pwsh).

Example:
```pwsh
> Import-Module .\Avanade.BizApps.Tools.DeploymentTool.Cmdlets.dll
> $connectionString = "..."
> Get-SolutionVersion -ConnectionString $connectionString -SolutionUniqueName "TEST20Customizing"

Major  Minor  Build  Revision
-----  -----  -----  --------
0      0      0      1
```

In order to get the available Cmdlets, use the following:

```pwsh
Get-Command -Module Avanade.BizApps.Tools.DeploymentTool.Cmdlets
```

Use `-InformationAction Continue` to display the standard and more exhaustive log messages in the PowerShell console.