# User Management

Managing user access in Dynamics is important and consumes time if done manually for each user. EMC tool helps to semi-automate the process with the help of XML configuration and an Excel which can be maintained centrally.

This scenario covers the following User Management activities
- Setting User attributes like Business Unit
- Assignment of Security Roles
- Assignment of Field Security Profiles
- Assignment of Team

## User Management Excel
- This is a centrally maintained excel with the users (who already have license and are active in the Dataverse environment).
- Excel tab "Instructions" provides details about each column and maintaining the excel in coordination with the XML structure.
- [CRMEMC_UserManagement.xlsx](./_assets/CRMEMC_UserManagement-72b70db6-a320-4710-91a9-445de8d0713a.xlsx)

## User Management XML Config
- The columns in the excel are mapped to the attributes, Security Roles, Field Security Profiles, Teams via this XML configuration
- It is very important to map the right columns with the right component in the XML config.
- Ensure that the correct naming is provided for the components (for example security role) to avoid any misconfiguration and wrong assignment of the roles.
- [CRMEMC_UserManagement.xml](./_assets/CRMEMC_UserManagement-a88813fc-9883-4f98-be61-fa8dd9cd5548.xml)
 
