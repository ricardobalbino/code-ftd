# Quick Start Guide

## First Install

See [here](../../Deployment-Tool/quick-start-guide/#first-install) for instructions to install the EMC and other tools.

## "How do I use this?" (Six-Step Setup)

In order to answer this question, simply execute the six steps mentioned below. If you want to dig deeper, extend the configuration to suit your own data setup or use the command-line functionality for extended automation, check out the other chapters.

1.  Copy the tool to an environment running Windows and from which you have access to your PowerApps/Dynamics environment via an Active Directory account.

2.  Modify `CRMEMC.App.exe.config` to provide your environment connection (use examples provided in the app.config to connect including OAuth or CRM on-premise):

3.  Run `CRMEMC.App.exe.`.

4.  Verify that `CRMEMC_Example.xlsx`  is located in the given directory or select it using the "Browse" button and click "Preload Template". This loads example data from the workbook `CRMEMC_Example.xlsx` into memory but not yet into the system.

5.  In order to avoid to import undesired data, click "Deselect All" and select only the highlighted "contact" entry.

6.  Click "Update Entities" which uploads one contact "test@example.com" to your  environment which you can delete afterwards.

## How to Configure and Refine Your Own Data Setup

Configuring your own set of data will be the next step to implement advanced data integration scenarios. It consists of several building blocks which are described in the further sections.

### Excel Structure and Contents

You can have one single Excel file or multiple, depending on your scenario. Example scenarios are described in the following. There are multiple other scenarios where the tool can be used. You can get inspiration on the structure of such an Excel workbooks looking at the example provided: `CRMEMC_Example.xlsx`

### Central Test or Configuration Data

If you want to establish centrally defined test or configuration data which can be distributed to multiple environments with a single click, then it is recommended to have a single Excel file containing all data which can for example be maintained and stored on a SharePoint. This workbook would have multiple sheets containing all the test data.

Once you want to publish a new set of data, you must modify this workbook and use it to transfer the data to all environments so that all data is consistent across those environments.

### Global Country Rollout

You can define an Excel workbook with a certain structure in order to gather information about configuration data required to be imported in a global rollout. You would receive multiple templates and can import the
workbooks one by one during the rollout process.

### Entity XML Configuration

-   Each Excel workbook needs to have a matching configuration mainly in order to map rows and columns to CRM entity fields.

-   Each configuration is laid down in an XML file located in the `EntityXml` subfolder.

-   You can name each XML file as you like, but it should have a  meaningful name matching the purpose of the file or the related Excel file(s).

-   Derive your XML from an existing configuration file for example from the `EntityConfiguration.xml` provided. This example configuration contains many template configurations for different use cases and column specifications.

-   Most of all, for each entity configuration you should consider the following:

    -   *<entity name="..."* &rarr; Matches schema name of targeted entity

    -   *<entity sheet="..."* &rarr; Matches sheet name in Excel

    -   *<entity key="..."* &rarr; Matches name of key column(s)

    -   *<entity keycolumn="..."* &rarr; Matches number of column where the main key is contained (this determines the criteria used to terminate the processing of a sheet once the cell is empty)

    -   *<entity startrow="..."* &rarr; Matches the number of the row from
        which the processing should start

    -   Create a *<column ...>* definition for each column you want to
        map. Here you need to know the schema name of each target
        attribute in your MS CRM.

## How to Automate Via the Command-Line

You can streamline your data integration with the command-line
functionality of the EMC:

```
CRMEMC.UI.exe -commandline [-connectionstring:<Connection String>] [-configuration:<Configuration XML
from 'Config' folder>] [-workbook:<Path to workbook to be imported>] [-stringreplacements:<search1|replace1;search2|replace2;...>] [-allowcleanup:<true|false>]
```

| Parameter                | Mandatory | Description                                                                                                                                                                                                 |
| ------------------------ | --------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| \-commandline            | Yes       | Mandatory to use the command-line functionality of the tool.                                                                                                                                      |
| \-connectionsstring      | Yes       | If the connection shall be established via a plain connection string, this can be passed as a standalone parameter without any specific switch.|
| \-configuration          | No        | Alternative entity XML configuration to be used for the import. The XML file must be located in the _EntityXml_ subfolder. If not provided, the default configuration defined in the _app.config_ is used. 
| \-workbook \| -importfile | No        | Alternative Excel workbook, CSV or XML to be used for the import. An absolute path should be provided. If not provided, the default Excel workbook defined in the _app.config_ is used.  
| \-stringreplacements     | No        | List of string replacements pairs in the form of search1|replace1;search2|replace2;... This can be used for example to replace values for different target environments.                                    |
| \-allowcleanup           | No        | Only to be provided with value “true” if a “delete” action has to be performed. Otherwise the deletion would not work. For all other actions this does not have any relevance.                              |
| \-proxy | No | Allows to set the default proxy (`WebRequest.DefaultWebProxy`) for the Dynamics/Dataverse connection | 
