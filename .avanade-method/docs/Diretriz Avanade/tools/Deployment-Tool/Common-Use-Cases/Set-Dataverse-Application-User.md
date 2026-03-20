# Set Application User

## Option 1: Configuration XML
In order to automatically set up an Application User in Dynamics and the related App Registration/Service Principal in the Azure Active Directory of Office 365 in one go, use [this configuration XML](https://dev.azure.com/innersource/DSS-Framework/_git/DeploymentTool?path=/DeploymentTool.Console/Xml/Examples/CdsSetup/SetUpApplicationUser.xml&version=GBmaster).

Run the tool with the following command, leave the switches out if you want to keep the default value:

```
dpa.exe SetUpApplicationUser.xml -OrgName <Name of the Dynamics environment> -Region crm4 -AppRegistrationName <Name of the App Registration/Service Principal, default: DynamicsApplicationUser> -ApplicationUserFirstname <First name of the Application User in Dynamics, default: Dynamics> -ApplicationUserLastname <First name of the Application User in Dynamics, default: Application> -ApplicationUserAddress <Email address of the Application User in Dynamics, default: crm.master@example.com> -BusinessUnitName <Name of the business unit for the Application User, default: Same as OrgName>
```

## Option 2: PowerShell and Cmdlets

```pwsh
<#
.SYNOPSIS
  This script creates a new Application User in the Azure AD and PowerApps/Dynamics.
.DESCRIPTION
  This script creates a new Application User in the Azure AD and PowerApps/Dynamics.
#>

[CmdletBinding()]
Param(
  [Parameter(Mandatory = $true)][string]$Orgname,
  [Parameter(Mandatory = $true)][string]$BusinessUnit,
  [string]$ConnectionString,
  [string]$Region = "crm4",
  [string]$AppRegistrationName = "DynamicsApplicationUser",
  [string]$ApplicationUserAddress = "dynamics.application@demo.com",
  [string]$ApplicationUserFirstname = "Dynamics",
  [string]$ApplicationUserLastname = "Application",
  [switch]$SkipGrantAdminConsent
)

function Set-AppPermission($ApplicationId, $ApiName, $PermissionName, $ScopeOrRole = "Scope") {
  $apiId = (az ad sp list --all --filter "displayname eq '$ApiName'" | ConvertFrom-Json).appId
  $permissionId = az ad sp show --id $apiId --query "oauth2Permissions[?value=='$PermissionName'].id" | ConvertFrom-Json
  az ad app permission add --id $ApplicationId --api $apiId --api-permissions "$($permissionId.Trim())=$ScopeOrRole"
  
  # In June 2021 it seemed that it was suddenly necessary to create a SP along with the app registration
  # but no longer necessary to grant admin content. This is to be observed.
  if (!($SkipGrantAdminConsent.IsPresent)) {
    az ad app permission grant --id $ApplicationId --api $apiId --scope $PermissionName
  }  
}

function New-AppRegistration {
  # Login to Azure
  az login --allow-no-subscriptions

  # Create app registration
  $applicationId = (az ad app create --display-name "$AppRegistrationName" | ConvertFrom-Json).appId
  az ad sp create --id $applicationId

  # Create secret for App sociated service principal
  $secret = (az ad sp credential reset --name $applicationId | ConvertFrom-Json).password

  # Set API permissions
  Set-AppPermission -ApplicationId $applicationId -ApiName "Microsoft Graph" -PermissionName "User.Read"
  Set-AppPermission -ApplicationId $applicationId -ApiName "Dataverse" -PermissionName "user_impersonation"

  return @{ "applicationId" = $applicationId; "secret" = $secret }
}

# Set up libraries
Import-Module "$PSScriptRoot\Shared\Scripts\Common.psm1" -Force
Install-Nuget
Import-Module (Join-Path -Path (Get-DeploymentToolFolder -ProjectRootPath $PSScriptRoot) -ChildPath "Avanade.DPA.Deployment.Cmdlets.dll")

# Create app registration
$appRegistration = New-AppRegistration

# Set up user in Dataverse
if ([string]::IsNullOrEmpty($ConnectionString)) {
  # If not defined, use standard connection string
  $ConnectionString = "AuthType=OAuth;  Username=me;  Integrated Security=true;  Url=https://$Orgname.$Region.dynamics.com;  AppId=51f81489-12ee-4a9e-aaae-a2591f45987d;  RedirectUri=app://58145B91-0C36-4500-8554-080854F2AC97;  TokenCacheStorePath=c:\\MyTokenCache;  LoginPrompt=Auto"
}

Set-TableRow `
  -TableName "systemuser" `
  -Key "internalemailaddress" `
  -Value "$ApplicationUserAddress" `
  -Values @{ "firstname" = $ApplicationUserFirstname; "lastname" = $ApplicationUserLastname; "internalemailaddress" = $ApplicationUserAddress; "applicationid" = $appRegistration.applicationId; "businessunitid" = $BusinessUnit } `
  -ConnectionString "$ConnectionString"

Grant-SecurityRole -UserName $ApplicationUserAddress -SecurityRoleName "System Administrator" -ConnectionString "$ConnectionString"

Write-Host "Your application user has been created. Use the following connection string:"
Write-Host "Url=https://$Orgname.$Region.dynamics.com/; AuthType=ClientSecret; ClientId=$($appRegistration.applicationId); ClientSecret=$($appRegistration.secret)"
```