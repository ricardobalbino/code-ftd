# Run Deployment Pipeline

The [deployment pipeline](../Pipelines/dataverse-deploy.md) to deploy one or several solutions to the upper environments is triggered **manually** to have highest control over the release process and to avoid clashes of multiple deployments triggered around the same time (see [Environment Approvals](../getting-started/next-steps/environment-approvals.md) for additional deployment gates).

!!! note

    When using managed solutions, you need to upgrade your solutions from time to time in order delete removed components from the target environment using the `Upgrade solutions` pipeline parameter. By default, solutions are deployed with update-only (Dataverse would throw an error when trying to upgrade a solution without component changes).

1. Log on to your project's Azure DevOps.
1. Go to `Pipelines`.
1. Go to the Dataverse Deploy pipeline.
1. **Optional:** If you need to upgrade your solution, enable the pipeline switch `Upgrade solutions`.
1. Click `Run`.
1. Select one or several solutions to be deployed.
