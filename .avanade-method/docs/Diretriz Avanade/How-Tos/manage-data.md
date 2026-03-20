# Manage Data

Avanade BizApps Core by default provides a streamlined and extensible way to handle data imports (supported by the [Entity Management Cockpit (EMC)](../../tools/Entity-Management-Cockpit)).

After the initial setup, data import of Configuration Data (to all upper environments) and Test Data (only to Dev-Int and Dev-Test) is already set up as part of the deployment pipelines.

The XML files `src\Dataverse\Data\ConfigurationData.xml` and `src\Dataverse\Data\TestData.xml` contain a lot of examples for out of the box tables and columns which can be enabled for import by commenting in the XML artifacts and changing the column structure.

Whilst handling of data in Excel and XML is also possible, the data is handled via CSV files by default because it is the most effective and easiest way to track changes and exchange data with other teams if required.

You can easily try out different import and export scenarios locally via the local environments which makes it really easy to modify data locally in the CSVs or shape them in Dataverse and then export for takeover into the repository. The CLI supports these workflows completely.

## Change data and configuration locally and import to lower environment

1. Go to `src\Dataverse\Data\ConfigurationData.xml` or `src\Dataverse\Data\TestData.xml` and make changes to the configuration and/or make changes to the `.csv` files in the respective subfolders.
1. Run `bizapps-cli.ps1`.
1. Select `ExportImportData`.
1. Select `Import Data`.
1. Select the xml file.
1. Select lower target environment.
1. Select "Yes" to proceed with the import.
1. Log on the lower environment and review your changes.
1. [Create Pull Request](create-pull-request.md) and commit the changes.

!!! info

    After commit, the data will be automatically imported to all upper environments.

## Change data in lower environment, export to local, commit to repository and deploy to upper environment

1. Log on to lower environment.
1. Modify any data which is configured in `src\Dataverse\Data\ConfigurationData.xml` or `src\Dataverse\Data\TestData.xml`. (If required, enable any data or add an <entity> node.)
1. Run `bizapps-cli.ps1`.
1. Select `ExportImportData`.
1. Select `Export Data`.
1. Select the desired .xml file. 
1. Select lower environment which has been used to modify the data.
1. Manually take over the record(s) to the corresponding .csv file in `src/Dataverse/Data/...`.
1. [Create Pull Request](create-pull-request.md) and commit the changes.

!!! info

    After commit, the data will be automatically imported to all upper environments.

## Generate XML schema

Instead of manually creating the XML schema, you can either review scenarios contained in the [EMC](../tools/Entity-Management-Cockpit/Scenarios/index.md) or you can use the CLI to create the column schema for one or several tables.

1. Run `bizapps-cli.ps1`.
1. Select `ExportImportData`.
1. Select `Generate Schema`.
1. Enter the regular expression for the schema to generate.
1. Select desired lower environment.

!!! info

    Your schema has been created and you can take the column information to create or extend the schema in the respective xml file in `src/Dataverse/Data`.

## Add Environment-Specific Data

!!! important "Data Security"

    This is only applicable to non-sensitive data. For secure data handling see [here](Others/add-secrets.md).

1. Open `Configuration.json`.
1. _Optional:_ Define the parameters and its default value in the `GlobalParameters` node (for example as `"MyParameter": "MyValue"`).
1. Add the parameter and its environment-specific value under each applicable environment node in `Environments` inside the `Parameters` node (for example as `"MyParameter": "MySpecificValue"`).
1. Add a placeholder with curly brackets ("`{MyParameter}`") in the .csv data files as needed. It will be dynamically replaced in the CLI runs as well as during the automated pipeline runs.

## Add Other Data

In case you need any data apart from configuration data or test data (for example rollout data), do the following:

1. Add a new XML file `<Data Name>.xml` in `src/Dataverse/Data`.
1. Add the respective XML schema to the XML file for all tables and columns you want to consider.
1. Add a new folder `<Data Name>` in `src/Dataverse/Data`.
1. Export the .csv data from a lower environment or alternatively create the .csv files inside the `<Data Name>` folder.
1. Add the value "`<Data Name>`" inside the "`DataFiles`" property (array) inside `Configuration.json`.

Your data will be automatically imported into all environments where "`<Data Name>`" is included in "`DataFiles`". (This allows for maximum flexibility, for example when you want to have different files per environment.)