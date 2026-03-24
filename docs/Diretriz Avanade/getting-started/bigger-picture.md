# Bigger Picture

Understanding the main technical principles and components behind the Avanade BizApps Core Accelerator is not required in order to work with the accelerator on a daily basis, but the background knowledge is helpful if you want to troubleshoot, extend or optimize parts of the accelerator in your asset, project or just generally want to know how things work.

## Components

### BizApps Platform

The [BizApps Platform](https://platform.avanade.com) supports an easy starting point for your project setup which provides a simple Web UI to set up a project without running a lot of scripts or performing manual tasks.

The platform only asks for some project metadata and preferences as well your connection information for Azure DevOps and your Dataverse environments to initialize the project repository with a modified clone of the [Template project](https://dev.azure.com/innersource/DSS-Framework/_git/Template) and to create the pipelines using the given connecting strings to run the automated deployments via the pipeline.

??? info

    The setup via the platform is the recommended way whereas technically a Template clone with adaptions will also work.

### Project Repository

You project is developed in one (Azure DevOps) Git repository which contains **everything** related to the solution you are building: Dataverse customizations, configurations, data and code.

Standard Git workflow concepts apply to manage branches, pull requests and commits.

The project structure is described [here](./overview/overall-structure.md).

### Pipelines

The artifacts are built and deployed from the repository using standard YAML pipelines as managed or unmanaged Dataverse solutions. Standard tools like [Power Platform CLI (pac.exe)](https://docs.microsoft.com/en-us/powerapps/developer/data-platform/powerapps-cli) are used but limitations are bridged by the BCA tools. Additionally, special Dataverse-related deployment automation tasks as well as data imports are built into the pipelines which are ready to be used (if not required, they can be safely ignored).

### Solution Segmentations

By default, the setup process via the platform offers different solution segmentation approaches (see [Solution Flows](./overview/solution-flows.md) for more information and references).

### Backend and Frontend Code

A **light-weight (low-code or no-code) scenario**  is completely supported (the mode can be selected during the setup process on the BizApps Platform) but also pro-code approaches using backend code (plug-ins, Custom APIs, etc.) or frontend code is possible by either selecting it during setup or later adding it via the CLI (see section below).

Backend and frontend components can be linked to the desired Dataverse solution but the code itself is grouped in the repository which accelerates code reuse but also provides functional isolation in the solutions. 

The pro-code library support is exhaustive (see [Development](../../development)).

### CLI

All common workflows in the daily life of a customizer, developer, tester or DevOps engineer are supported by the `bizapps-cli.ps1` CLI. This means that no extra tooling or manual takeover is required.

The CLI embraces `pac.exe` but again like in the YAML pipelines (using the same PowerShell libraries) augments and bridges gaps in the Microsoft tooling to shield team members from making manual mistakes and reduce the number of manual steps.

### Configuration.json

The `Configuration.json` stores the main metadata of the project in one place which are the publisher and solution information as well as environment information. This file does not have to be managed manually as the CLI handles this for the most part but in some advanced scenarios it is helpful to understand and modify the JSON. 

### NuGet Feed

The [Avanade DSS feed](https://dev.azure.com/innersource/DSS-Framework/_packaging?_a=feed&feed=DSS) is used to retrieve the relevant NuGet and npm packages to build the code and to get the relevant tools from. Version updates can be done in the same way as with any package managers (NuGet / npm).

Using the [Eject script](../How-Tos/Others/administer-project-repository.md#execute-eject-script), the NuGet feed connection can be cut off and all packages related code is moved into the current repository which utilizes the leave behind scenarios.

### Environments

The below table describes the list of recommended and optional environments used in an engagement/asset.

| Environment | Alias | Purpose | Usage Recommendation
| --- | --- | --- | --- | 
| Dev | | Push code developed in Visual Studio (Code) and to try it without impacting or tampering with ongoing customizations. | Recommended for all projects but can be clubbed with CM in small projects or in no-code/low-code scenarios |
| Customization Master/Main | |  Handle Dataverse customizations, data,  backend and frontend code registrations to allow sync to repository. | Required
| Dev-Int | | Artifacts are built and published automatically after commit. Used by developers to verify certain implementations in combination with customizations before carrying out user tests in upper environments. Can also be helpful to run integration tests with or without integrations or automated UI tests. | Recommended for projects
| Dev-Test | SIT, QA | Deploy artifacts (which might have been tested automatically in Dev-Int) and execute the manual iteration tests. (In Microsoft Dataverse implementation guide this environment is referred to as QA.) | Can be skipped for assets or internal projects
| UAT | Pre-Prod | Verification by the client (internal or external) | Required
| Prod | | | Productive deployment | Required
| Showcase | | Allow pre-view access to external users | Recommended for assets
| Training | | Training of new users | Optional. Recommended if a larger number of users has to be trained on new releases or as a roll-out preparation.
