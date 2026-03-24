# Create a Pull Request

After changes in your local branch have been committed and pushed, execute the following steps.

1. Create a Pull Request in Azure DevOps to merge your user branch into the master branch.
1. Add a descriptive title and a precise description describing the changes made.
1. Add the relevant work item references (if commit message contain PBI/US information e.g. `#12345` the list of relevant work items is already populated)
1. If required: Add the required minimum number of reviewers.
1. Work on the comments and amend the Pull Request as needed.
1. Once approved, complete the pull request.

!!! info

    Any changes to the code area (`src/Dataverse/Backend`, `src/Dataverse/Web` and `src/Dataverse/PCF`) will automatically trigger the [deploy code pipeline](../Pipelines/dataverse-deploy-code.md) deploying the artifacts to Customization Master.

    The general [deploy pipeline](../Pipelines/dataverse-deploy.md) is triggered manually which gives developers and operations full control over what is deployed and when instead of it triggered with every commit (the pipeline also increases the solution version to make sure that the versioning strategy is observed consistently). 
