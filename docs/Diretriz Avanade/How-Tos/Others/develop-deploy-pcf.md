# Develop and Deploy a PCF Component

See [Development Guidelines](../../development/PCF/index.md) for general guidance on PCF.

## Add new PCF component to existing solution	

1. Run `bizapps-cli.ps1`.
1. Select `NewPcfComponent`.
1. Select solution to add PCF component to.
1. Enter component name.
1. Review repository.

## Develop PCF component locally, push to dev and commit.	

1. In your current shell, go to PCF component folder.
1. Run `npm start`.
1. Run `pac pcf push` (authenticate to dev environment).
1. Commit the changes.
1. Review PCF in customization master.

## Add PCF component in form customization and deploy to upper environment	

1. Log on to Customization Master.
1. Add PCF component to any form in a given solution.
1. Run `bizapps-cli.ps1`.
1. Select `UnpackSolution`.
1. Select the solution modified above.
1. Review repository.
1. [Create Pull Request](../create-pull-request.md) for customization changes.
