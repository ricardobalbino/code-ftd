# Data Management

The manangement of data outside the typical (Azure) integration of master and transactional data is essential in a Dataverse solution.

The **BizApps Core Accelerator** supports streamlined data management with configuration data, test data and other data being automatically deployed into the designated target environments by the pipelines as well as with CLI actions to quickly prepare and extract data from lower environments or try out certain configurations quickly.

All relevant data is centrally defined in the repository.

## Configuration Data

Configuration data is always imported into all environments.

By default, only the Bulk Deletion Job for Command records is enabled, other configuration data can be easily added:

1. When example data already exists: Modify the .csv file and comment in the respective `<entity>` node in `src/Dataverse/Data/ConfigurationData.xml` (see [here](../../How-Tos/manage-data.md#change-data-and-configuration-locally-and-import-to-lower-environment))

1. When the data does not exists and first has to be exported: Set up the data in Customization Data or Dev, export it and take over the needed records to the .csv file in `src/Dataverse/Data/ConfigurationData` (see [here](../../How-Tos/manage-data.md#change-data-in-lower-environment-export-to-local-commit-to-repository-and-deploy-to-upper-environment)).

## Test Data

Test Data is typically imported into the Dev-Test (SIT) environment, sometimes also in UAT when connected interfaces cannot provide the UAT data.

Test data falls in line with the agreed data model and typically concentrates around production-like but surely anonymized data
provided by the stakeholders.

Once your data model is known and ready to a reasonable extent, you can apply the follow process to set up the data. Please note
that there are of course variants to this process, this is just a suggestion:

1. Identify the Tables for which master data is available.
2. Create the XML schema in `src/Dataverse/Data/TestData.xml` as described [here](../../How-Tos/manage-data.md#generate-xml-schema).
3. Create test data in any lower environment.
4. Export, modify and commmit test data as described [here](../../How-Tos/manage-data.md#change-data-in-lower-environment-export-to-local-commit-to-repository-and-deploy-to-upper-environment).

After the commit, the test data is automatically imported upstream to Dev-Int (if available) and Dev-Test and all other environments where the `DataFiles` node contains "TestData" in `Configuration.json`.

## Other Data

Other data which is not managed by any integration components, for example Rollout Data like Queues, Users, etc. can be added as described [here](../../How-Tos/manage-data.md#add-other-data). The data can be configured differently for each environment using the `DataFiles` property in `Configuration.json`.

## Add Environment-Specific Data

Environment-specific data can be handled in the following ways:

* [Unsecure data](../../How-Tos/manage-data.md#add-environment-specific-data)
* [Secure data](../../How-Tos/Others/add-secrets.md)

## Further Information

Please check the [Entity Management Cockpit Wiki](../../tools/Entity-Management-Cockpit/index.md) on further topics, for examples

* Use predefined schema XMLs (Field Service, Marketing, etc.)
* Use special configuration attributes for example to import N:M relationships