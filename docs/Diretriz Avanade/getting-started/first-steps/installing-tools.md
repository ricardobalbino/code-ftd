# Installing Tools

This pages provides all the necessary information and guidance to setup your local machine and be ready to dive into your engagement.

## Prerequisites

On a typical Avanade or Accenture laptop, it should be common to have both (Visual Studio and VS Code) IDEs. If not, please make sure to install them before continuing with the guide below. Whereas it is also possible to use only the command-line or certain Git tools to maintain your project artifacts locally, it is recommended to use the IDEs below.

!!! note

    For the majority of the workflows, Visual Studio Code is completely sufficient and this is recommended for anything not related to backend code due to the easy way to install it, quick startup time and extension support. This also means that Visual Studio is not required for a light-weight setup.

- **Visual Studio Code**  
    * Download from: [https://code.visualstudio.com/](https://code.visualstudio.com/)

- **Visual Studio Professional 2022** or **Visual Studio Enterprise 2022** (Visual Studio 2019 should also be fine as of December 2021 where we are easing into the new 2022 version)
   
    * Download from: [https://visualstudio.microsoft.com/downloads/](https://visualstudio.microsoft.com/downloads/)  
    * Please make sure you have .NET Framework versions `4.6.2` and `4.7.2` installed.  
    * In case you do not have a license, please request one through the IT service.

## Install Tools

The BizApps Core Accelerator requires you to install the below tools (You can choose to install the tools on your own or use the CLI. Please see the section below):

- **Git**  
   Git is used for source control and usually comes bundled with Visual Studio, but we require a dedicated installation.

- **NodeJS**  
   NodeJS is required for frontend development. We recommend to have the latest current or LTS version installed.

- **Azure Artifacts Credential Provider**  
   Used for local authentication to the Innersource NuGet feed. More information available [HERE](https://github.com/microsoft/artifacts-credprovider).

- **Fiddler (Classic)**  
   Used for local webresource / formscript development. More information available [HERE](https://www.telerik.com/fiddler/fiddler-classic).

- **Microsoft Power Platform CLI**  
   PAC is used to interact with Dynamics instances from your local machine. More information available [HERE](https://docs.microsoft.com/en-us/powerapps/developer/data-platform/powerapps-cli).

- **BizApps Core Accelerator Visual Studio Extension**  
   This is optional, but we highly recommend it for new users of the BizApps Core Accelerator.  
   The Visual Studio extension provides various templates for backend and frontend development.  
   More information can be found [here](../../development/Visual-Studio-Extension.md).

### Automatic Installation using BizApps CLI

If you choose to install the tools automatically using `bizapps-cli.ps1`, you can follow the steps below.

1. Clone your project repository from Azure DevOps using for example [Visual Studio](https://docs.microsoft.com/en-us/azure/devops/repos/git/clone?view=azure-devops&tabs=visual-studio).
1. Open up Windows Explorer and navigate into the cloned project.  
   You should see a `bizapps-cli.ps1` file. If not, check that you have cloned the repository correctly.
1. Open up a Windows Powershell as Administrator and navigate into the same folder (in Visual Studio Code you can do this easily from the terminal).
1. Execute the following command `#!pwsh Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope LocalMachine -Force` to allow the CLI to be executed.
1. Execute the CLI by typing `#!pwsh .\bizapps-cli.ps1` and hitting ++return++.
   You will be prompted with all available actions of the CLI. Some of them will be included in your day-to-day workflows with the BizApps Core Accelerator, so this first step already provides you with valuable experience in working with the CLI (see [here](../../How-Tos/index.md) for all other supported CLI actions).
1. Select the option `SetupLocalDevEnvironment` by typing ++0++ and hitting ++return++.  
   The CLI will download and install NodeJS, Git and the Visual Studio extension.
   This process will take a few minutes.
