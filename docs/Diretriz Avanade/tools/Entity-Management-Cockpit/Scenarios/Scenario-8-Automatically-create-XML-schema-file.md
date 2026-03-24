# Automatically create XML schema files

# tl;dr
For a long time, all the schema XMLs for the EMC had to be either copied from existing configurations, created from scratch or derived from Advanced Finds (for the advanced users). Now there is:
1. a simple ["library XML"](https://dev.azure.com/innersource/DSS-Framework/_git/EntityManagementCockpit?path=/EntityManagementCockpit.App/EntityXml/Scenario8_AllTablesSchemaLibrary.xml&version=GBmaster&_a=contents) which has all the XML configurations for >900(!) tables/entities
2. a simple Command in the Deployment Tool which allows you to quickly create the schema XML for all or selected entities.

# Steps
## Option 1 - Use library XML
1. Review [library XML](https://dev.azure.com/innersource/DSS-Framework/_git/EntityManagementCockpit?path=/EntityManagementCockpit.App/EntityXml/Scenario8_AllTablesSchemaLibrary.xml&version=GBmaster&_a=contents).
1. Modify XML as needed.
1. Use XML to export or import data via the EMC.

## Option 2 - Create your own XML via BCA
See [here](../../../How-Tos/manage-data.md#generate-xml-schema) on how to create a schema XML file via `bca-cli.ps1` (this script uses the `New-XmlSchemaForEmc` Cmdlet of the DPA).

## Option 3 - Create your own XML via DPA XML Configuration
1. Configure and run the [DPA Command `CreateXmlSchemaForEmc`](https://dev.azure.com/innersource/DSS-Framework/_git/DeploymentTool?path=/DeploymentTool.Console/Xml/Examples/EntityManagementCockpit/CreateXmlSchemaForEMC.xml) to export the schema, for example into the file `schema_default.xml`. 
1. Review schema file.
1. Modify schema file as needed.
1. Use schema file to export or import data via the EMC.
