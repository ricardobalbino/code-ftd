# Portal Deployment

The streamling of Power Apps Portals will be included in BCA version 3.1 but it is already possible to include following these directions:

1. Identify a location for the Portal artifacts in your repository.
1. Configure your poral in Customization Master.
1. Use [Power Platform CLI for Portals (pac.exe)](https://docs.microsoft.com/en-us/powerapps/maker/portals/power-apps-cli) to download a Power Apps Portals website and store the artifacts in the repository.
1. Set up the deployment applying one of these options:
    * Adapt `deploy-generic.yaml` to upload the website from the repository
    * Create a dedicated pipeline to upload the website
    * (Not recommended, only in case of extreme time pressure or technical issues) Upload the website manually to the upper environment using `pac.exe`