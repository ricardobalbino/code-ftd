# Administer Project, Repository and Pipelines

Administration tasks to maintain the project repository with its package references, PowerShell scripts and YAML pipelines occur relatively rarely but are largely automated via the `bizapps-cli.ps1` to streamline and simplify the required steps.

## Update Packages

The packages for backend and frontend can be updated in the following way:

1. **NuGet**: Use NuGet Manager or similar to update the respective package version in `src/Tools/Dataverse.Framework.Tools.csproj`.

1. **npm**: Use `npm outdated` or `npm update` in `src/Web` (see also [here](../../../How-Tos/Others/manage-npm-packages) for more details how to handle npm specifics).

## Update CLI Scripts

As the PowerShell scripts are not part of the NuGet packages and reside in the Git repository, updates can be pulled in on demand via the CLI script.

1. Run `bizapps-cli.ps1`.
2. Select `UpdateCLI`.

!!! info

    All scripts in `cli` are updated and can be committed to the repository. (In case any manual adaptions need to be kept, you can easily do this via a manual Git merge.)

## Update YAML pipelines

As the YAML pipelines are not part of the NuGet packages and reside in the Git repository, updates can be pulled in on demand via the CLI script.

1. Run `bizapps-cli.ps1`.
2. Select `UpdateYAML`.

!!! info

    All pipeline files under `ci-cd` are updated and can be committed to the repository. (In case any manual adaptions need to be kept, you can easily do this via a manual Git merge.)

## Execute Eject Script

The eject script supports the important "Leave-Behind Scenario" which might become relevant when in a project the Avanade team hands over the repository to the client or a 3rd party (requires an NDA to be in place). The script automatically cuts off the project repository and pipelines from the Avanade package feeds and leaves all code behind to be maintained (or left as-is) by the receiving team.

Eject scripts are a common approach in the application development space and leave behind is a common scenario in projects. With the eject script of the Avanade BizApps Core, Avanade does not leave behind a proprietary project structure and artifacts which are are limited to the project scope and oftentimes with a lot of structural technical depths and a lack of extensibility. The team rather leaves behind a well-maintained, extensible and very workable structure with all source code open for the next team to modify and extend.

??? note "Impact"

    Logically, once the eject script has been executed, updates from the Avanade BizApps Core cannot be automatically received anymore.

1. Run `bizapps-cli.ps1`.
1. Execute `EjectTools`.

!!! info

    The script does the following:

    - Pull all tools into `src/Tools`
    - Remove Avanade feed references from `NuGet.config`.

1. Run `bizapps-cli.ps1`.
1. Execute `BuildTools`.

!!! info

    The script automatically builds all tools.
