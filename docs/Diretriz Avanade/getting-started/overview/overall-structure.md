# Overall Structure

If you have not done so already, we recommend you to clone your project's source code and install the required [tools](../first-steps/installing-tools.md#automatic-installation-using-bizapps-cli). This guide breaks down the complete folder structure and provides information for each area. In the process, you will get to know some important files that you will likely need/modify/extend in the future.

## Folder Structure

In the following tree you can see the relevant files and their description.

```text
example-project
  +-- [cicd]                         Contains the YAML files that define your CI/CD pipeline.
  +-- [cli]                          Contains the different CLI modules. Usually, this is of not much interest and can safely be ignore.
  +-- [feed]                         Used to store custom NuGet or npm packages for local reference. This can safely be ignored and is only relevant in a few special use cases.
  +-- [src]                          Contains all your source code which is explained in detail below.
  +-- bizapps-cli.ps1                The BizApps Core Accelerator CLI that is used to automate several actions.
  +-- info.json                      Do not touch, only used by tools and the BizApps Platform.
  +-- LICENSE.md                     The license file of the BizApps Core Accelerator and all its associated toolings.
  +-- NuGet.config                   Defines the NuGet feeds that are available in Visual Studio. Usually, this file does not need to be touched.
  +-- example-project.sln            The solution file containing your backend code (the front end code is handled without a project to avoid issues with Visual Studio frontend project types).
  +-- example-project.code-workspace The VS Code workspace which can be used to scope the VS Code instance to the frontend part of the BCA
```

## The `src` Folder

The `src` folder contains all your source code as well as some deployment files.

```text
src
  +-- [Dataverse]
  |     +-- [Backend]       Optional (if enabled): Contains all your backend code that is used to develop plug-ins. If backend is enabled for a solution, there will be one plug-in in "src/Dataverse/Plugins/Dataverse.Plugins.<SolutionName>".
  |     +-- [Data]          Contains configuration data, test data and potential other data to be automatically deployed to the environments.
  |     +-- [Deployment]    Contains the pre and post steps for automated deployments via Deployment Tool (DPA) as well as external managed solutions to be deployed alongside the project solutions.
  |     +-- [Solutions]     Contains the .cdsproj files for the solutions as well as the unpacked solution artifacts (customizations) which are referenced by the .cdsproj projects and which are used to build, pack and deploy the actual Dataverse solutions.
  |     +-- [Web]           Optional (if enabled): Contains the web project and all your code that is used for Form Script and Web Resource development.
  +-- [lib]                 Mostly empty in the beginning. Only relevant for the eject function at the end of an engagement.
  +-- [Tools]               Contains an empty Visual Studio project that is used to track the referenced tools and components as NuGet packages.
  +-- Configuration.json    The main configuration file of the BizApps Core Accelerator.
```
