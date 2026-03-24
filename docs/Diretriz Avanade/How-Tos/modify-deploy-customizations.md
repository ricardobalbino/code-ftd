# Modify and Deploy Customizations

The following is probably the most common workflow. Customizers or developers and making changes in the Customization Master environment, create a pull request and the changes get deployed automatically to the upper environments.

!!! important

    If you are not able to access the video stream below, please use [this alternate link](https://avanade.sharepoint.com/sites/DSSOfferings/Shared%20Documents/Forms/AllItems.aspx?id=%2Fsites%2FDSSOfferings%2FShared%20Documents%2FDSS%20Framework%2F300%20Tutorials%2FModify%20and%20Deploy%20Customizations%2Emp4&parent=%2Fsites%2FDSSOfferings%2FShared%20Documents%2FDSS%20Framework%2F300%20Tutorials).

<div style='max-width: 1280px'><div style='position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden;'><iframe width="1280" height="720" src="https://web.microsoftstream.com/embed/video/84663f70-11e9-48e4-be3f-b7cf70e6d61c?autoplay=false&showinfo=true" allowfullscreen style="border:none; position: absolute; top: 0; left: 0; right: 0; bottom: 0; height: 100%; max-width: 100%;"></iframe></div></div>

1. Log on to https://make.powerapps.com and select `Customization Master` environment.
1. Open any solution related to your project (hint: this must be configured in `Configurations.json`).
1. Make the necessary changes to the customizations as by the Customizing Guidelines.
1. Run `bizapps-cli.ps1` on your local development environment.
1. Select `UnpackSolution`.
1. Select the solution (must be the one which you modified in Dataverse before).
1. Review the changes after execution.
1. Create a new feature branch
1. Commit the changes (make sure to add the relevant PBI/User Story id to the commit message using `#` and the number e.g. `#12345`)
1. Push the changes
1. [Create a pull request](../create-pull-request).

!!! important
    
    In order to deploy your changes, you must run the [deploy pipeline](run-deploy-pipeline.md).
