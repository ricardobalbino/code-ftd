The Configuration Migration Tool is provided by Microsoft and it is free. It helps to move configuration
data across environments and environments. The following table shows the advantages and limitations in comparison to the Entity Management Cockpit Tool.

| **#**  | **Features**                                                                                                                                                                 | **EMC** | **CMT** |
| :------: | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :-------: | :-------: |
| **1**  | Automatic creation of schema in XML                                                                                                                                          | -      | X       |
| **2**  | Exports data based on XML schema (EMC: Excel file; CMT: zipped Solution file)                                                                                                | X       | X       |
| **3**  | Imports data based on XML schema (EMC: Excel file, CSV, XML; CMT: zipped Solution file)                                                                                      | X       | X       |
| **4**  | Imports entities in the correct order based on XML schema                                                                                                                    | X       | X       |
| **5**  | Imports related records in the correct order automatically                                                                                                                   | X       | X       |
| **6**  | Imports M:N relationships                                                                                                                                                    | X       | X       |
| **7**  | Preserves existing GUIDs in target destiny like in source destiny during import                                                                                              | X       | X       |
| **8**  | Import of file attachments                                                                                                                                                   | X       | X       |
| **9**  | Imports of users and teams including association of team memberships, field security profiles and security roles (EMC: Editing in Excel; CMT: Editing XML files in Zip File) | X       | (X)     |
| **10** | Updates existing records based on matching criteria                                                                                                                          | X       | X       |
| **11** | Creates new entities and attributes                                                                                                                                          | X       | -      |
| **12** | Check if data is already available (via button)                                                                                                                              | X       | -      |
| **13** | Matches option sets like Status and Status Reason based on number                                                                                                            | -      | X       |
| **14** | Matches option sets like Status and Status Reason based on label                                                                                                             | X       | -      |
| **15** | Detailed information on each imported record                                                                                                                                 | X       | -      |
| **16** | Combined lookup keys to identify existing records for updates                                                                                                                | X       | X       |
| **17** | Imports Advanced Find Queries                                                                                                                                                | X       | -      |
| **18** | Bulk activation/deactivation                                                                                                                                                 | X       | -      |
| **19** | Bulk assignments or shares                                                                                                                                                   | X       | -      |
| **20** | Creation/update and scheduling of Bulk Deletion jobs                                                                                                                         | X       | -      |
| **21** | Entity clean-up                                                                                                                                                              | X       | -      |
| **22** | Creation of test data in Excel (without CRM)                                                                                                                                 | X       | -      |
| **23** | Multi login to different environments and data import in one UI session                                                                                                      | -      | -      |
| **24** | Running on Command Line                                                                                                                                                      | X       | -      |
| **25** | Designed to move very large quantities of data                                                                                                                               | -      | -      |

**Conclusion:** If a simple and quick solution is needed to move (configuration) data from A to B then the Microsoft CRM Configuration Migration Tool is a good choice. It provides a good set of tools to mange the data transport. But if more complex operations are needed, like bulk activities, creation of new data, entity clean-up and especially automatization based on command line, then the CRMEMC is a much more powerful tool which should be the first choice.
