# Develop and Deploy Frontend Scripts and Web Resources

The following flow is relevant for all web resource developments.

1. Open Visual Studio Code.
1. Pull the latest commits from the master branch.
1. Were customizations being done? -> [Generate early bound classes](../development/Frontend/Early-Bound-Entity-Classes.md) or [Generate early bound forms](../development/Frontend/Early-Bound-Forms.md)
1. Develop code under `src/Dataverse/Web/src/entity/**`. See [Frontend](../development/Frontend/index.md) for more details and [Development Guidelines](../development/Guidelines/index.md) for guidance.
1. Create unit tests. See how to write frontend unit test [here](../development/Frontend/Testing/Writing-JavaScript-Tests.md).
1. Run `bizapps-cli.ps`.
1. Select `LocalDeployment`.
1. Select `Frontend`.
1. (Your changes are pushed to the Dev environment.)
1. Verify implementation in Dev.
1. Create and handle the pull request as described [here](create-pull-request.md).

The following flow is only relevant when a new web reosurce is added to a form because in this case, you need to change the form configuration in the Customization Master environment.

1. Wait until the changes from above have been automatically deployed to Customization Master. This will result in the web resource being available in the environment.
1. Make sure that the web resource is added to the correct solution.
1. If it's a form script: Make sure to add the web resource to the form and add the onload event.
1. Run `bizapps-cli.ps`.
1. Select `UnpackSolution`.
1. Review the changes in the unpacked solution xml (form script added to form & onload event present?)
1. Create feature branch (if not already existing)
1. Commit the changes (make sure to add the relevant PBI/User Story id to the commit message using `#` and the number e.g. `#12345`)
1. [Create Pull Request](create-pull-request.md) for customization changes.

!!! important
    
    In order to deploy your changes, you must run the [deploy pipeline](run-deploy-pipeline.md).