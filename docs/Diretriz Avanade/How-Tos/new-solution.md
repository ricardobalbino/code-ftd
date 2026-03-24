# New Solution

1. Run `bizapps-cli.ps`.
1. Select `NewSolution`.
1. Follow the instructions to determine the publisher:
   
    * For projects you should be fine using the default Publisher (which is defined during the initial project setup and then stored in `Configuration.json`).
    * For assets and in some specific cases, you may want to define a dedicated Publisher (see the Customizing Guidelines for background and further information).

1. Enter the display name of the new solution (all words must start with upper case, for example "Case Management").
1. Confirm if you want to add a plug-in for the solution.

??? note

    You can start without a plug-in if you are not sure if you want to create backend code for the solution and enable plug-in support at a later stage using the CLI.)
   
??? info

    The script uses the Power Platform CLI to create a new blank solution, inserts the Publisher information, optionally sets up a plug-in folder in `Dataverse/Backend/Plugins` and finally builds and imports the solution into the Customization Master environment so that the solution can be customized and used right away as described in [Modify and Deploy Customization](../How-Tos/modify-deploy-customizations.md)).

1. [Create Pull Request](create-pull-request.md) for customization changes.
