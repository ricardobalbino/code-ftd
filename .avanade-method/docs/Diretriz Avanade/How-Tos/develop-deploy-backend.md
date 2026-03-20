# Develop and Deploy a Plugin

The following flow is relevant for all plug-in developments.

1. Pull the latest commits from the master branch.
1. Open Visual Studio.
1. Were customizations being done? -> [Generate early bound](../development/Backend/Early-Bound-Entity-Classes.md) classes.
1. Develop code in respective solution's plugin project under `src/Dataverse/Backend`. See [How to Write a Plugin](../development/Backend/Plugins/Write-a-Plugin.md) for more details and [Development Guidelines](../development/Guidelines/index.md) for guidance.
1. Create unit tests for all relevant classes which have changed or where newly added (Application, Data, etc.) and use the standard Code Coverage Analysis of Visual Studio to determine where code coverage is missing.
1. Run `bizapps-cli.ps`.
1. Select `LocalDeployment`.
1. Select `Backend`.
1. Select solution for which the plug-in has been modified.
1. (Your changes are pushed to the Dev environment.)
1. Verify implementation in Dev.
1. Create and handle the pull request as described [here](create-pull-request.md). Note that your changes will be automatically deployed to Customization Master.

The following flow is only relevant when your plug-in changes result in a plug-in registration change (for example a new step is being added) because in this case, you need to add the plug-in registration step changes to the solution in the Customization Master environment.

1. Wait until the changes from above have been automatically deployed to Customization Master. This will result in the plug-in step changes being available in the environment.
1. Add newly created plugins/-steps to the correct solution
1. Run `bizapps-cli.ps`.
1. Select `UnpackSolution`.
1. Review the changes in the unpacked solution xml (new plugin/-step available?)
1. Create feature branch (if not already existing)
1. Commit the changes (make sure to add the relevant PBI/User Story id to the commit message using `#` and the number e.g. `#12345`)
1. [Create Pull Request](create-pull-request.md) for customization changes.

!!! important
    
    In order to deploy your changes, you must run the [deploy pipeline](run-deploy-pipeline.md).