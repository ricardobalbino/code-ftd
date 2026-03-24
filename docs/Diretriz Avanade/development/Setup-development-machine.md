# Setup Development Environment

!!! error "Obsolete"

    Replaced by [Installing Tools](../getting-started/first-steps/installing-tools.md)

Before digging into the code itself you should validate if your workstation has everything installed to work with the Framework effectively.

## IDE

### Visual Studio (VS 2019)

[Visual Studio](https://visualstudio.microsoft.com/) is the recommended IDE for developing the .NET/C# part. This includes __Plugins__, __Workflow, Actions__ and __Commands__.

Because of certain features like [Packages References](https://docs.microsoft.com/en-us/nuget/consume-packages/package-references-in-project-files) you have to have VS 2019+. Please make sure to install the appropriate version.

If you do not already have __Visual Studio__ installed go ahead and download it from the [Official Website](https://visualstudio.microsoft.com/). The default settings can be used for the installation.

### .NET Framework
Occasionally the local installation is missing the required .NET Framework 4.6.2 version. You can download it [here](https://dotnet.microsoft.com/download/visual-studio-sdks?utm_source=getdotnetsdk&utm_medium=referral).

### PowerShell

Because all provided PowerShell scripts are not digitally signed you have to enable the execution of such scripts once. Please run the following script in a PowerShell window.

`Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy Bypass`


### Visual Studio Code

[Visual Studio Code](https://code.visualstudio.com/) is an extensible and customizable text editor with support for many programming languages. It has very good JavaScript support and offers a lot more supporting functionality to the user. It supports ES2018 syntax and provides IntelliSense autocompletion and other user supporting features. 

__VS Code__ can be downloaded from [HERE](https://code.visualstudio.com/Download). The installation process is straight-forward and the default settings can be used.

## Tools

In order to get started developing frontend code you have to download and install a few additional tools.

>**To ease the process setting up your development machine you can simply run the dss-cli.ps1 in the repository's root folder. Simply execute it as an administrator and select the option "SetupLocalDevEnvironment".**

>**The following external tools will be installed during the execution of the above mentioned script.**

### git

For local deployments and the entity generation we need to determine the current active branch to find the correct entry in the Configuration.json. We achieve this by evaluating git commands. Please download the newest git version from [here](https://git-scm.com/).

### npm

We use [npm](https://www.npmjs.com/) as our package manager, which is part of NodeJS which you can download [here](https://nodejs.org/en/). If you have it already installed make sure to upgrade to the latest version. 

### Visual Studio Extension

BizApps Core Accelerator Templates are getting added to simplify creation of new components (e.g. Plugin classes). The installation is currently running silently and asynchronous in the background. Depending on the performance on your workstation this may take up to 5 minutes to finish. 

You can read more about the templates [here](Visual-Studio-Extension.md).
