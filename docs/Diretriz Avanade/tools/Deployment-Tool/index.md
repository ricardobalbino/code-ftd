# Deployment Tool (DPA)
The Deployment Tool streamlines and simplifies all sorts of application deployments focusing on Dataverse/Dynamics CE. Its main building blocks are dynamic Commands which are specified in a Deployment Configuration class which can be serialized and deserialized using an XML schema or alternatively used as PowerShell Cmdlets.

The tool shall not be seen as a competition to Azure DevOps (PowerApps Build Tools or other) but rather complementary in order to allow for a quick ramp-up time for new projects and to have a simple way for extensions (e.g. when new Commands or quick deployment configuration changes are needed).

# What's new?

Release 3.0.12:
  * Added Command `RestoreEmptyColumnsFromAudit` and `RestoreEmptyColumnsFromAuditCmdlet` to restore content from accidentally cleared columns from the audit history.

Release 3.0.11:

  * Replaced QueryByAttribute with QueryExpression in all main retrievals which fixes an issue querying Virtual Tables (https://markcarrington.dev/2021/04/21/querying-virtual-entities/).

Release 3.0.10:

  * Bumped EMC version to include fix for single .csv file imports.

Release 3.0.9:

  * Replaced ExecuteAsyncrequest with ImportSolutionAsyncRequest for asynchronous solution imports.

Release 3.0:

  * Fixed issue with solution comparison in scenarios where the last digit is greater than 1000.
  * Removed `FakeXrmEasy` from unit tests and moved to a pure mock scenario.
  * Added support to import and export of all CSV files from and to a folder including updates to the hash handling (via EMC).
  * Fixed problem with some scenarios where `AddWebResourceDependency` did not work.
  * Aligned naming to `Avanade.BizApps.Tools` namespace.
  * Added Command `CreateSecurityRoleAccess` and Cmdlet `New-SecurityRoleAllAccess` to quickly create a Security Role which has full access to all provided tables (which avoids the need to use System Administrator).
 
Release 1.13.0:

  * Renamed CRM references to Dataverse.
  * Added `GetSystemSettings` Command.
  * Added `GetUserSettings` Command.
  * Added `ExportSolutionAsync` Command.
  * Updated `SetTableRow` Cmdlet to support Hashtable input for value.
  * Improved retrieval of users for Assign and Revoke of Security Roles and FLSPs.
  * Added Command `CreatePolymorphicLookupAttribute` and corresponding Cmdlet `New-PolymorphicLookupAttribute` to create a multi-table lookup (not supported by PowerApps UI as of July 2021).
  * Added logic to Publish and Import Solution Commands to wait until are running operations are completed before continuing.
  * Increased EMC version to support Choices fields in data import and export as well as exporting and importing all CSV files from and to a folder.
  * Improved `ExportPluginRegistration` Command to included the actual DLL file and Guids.
  * Improved `RegisterOrUpdateAssembly` Command to consider Guids if available in order to support 1:1 plug-in migrations.
  * Added Command `ValidateSolutionCompleteness` and Cmdlet `Test-SolutionCompleteness` to compare consolidated solution components against individual solutions.
  * Added `DueDateFieldName` property to `CreateSprintPlan` Commands.
* Release 1.12.0:
  * Added `CreateXmlSchemaForEmc` to create an XML schema for data import or export with the EMC.
  * Added possibility to use a Global Parameter in any Command property which is empty and of type string (to avoid to redundantly configure for example the connection string in multiple Commands).
  * Copied `GetCrmVersion` to `GetDataverseVersion`.
  * Added `RunBenchmark` to  be able to run a benchmark on Dataverse and store the result in JSON.
  * Added first version of PowerShell Cmdlets for DPA.

# Feature Highlights

[List of Commands](https://dev.azure.com/innersource/DSS-Framework/_git/DeploymentTool?path=/DeploymentTool.Doc/Commands.md&version=GBmaster&_a=preview)

* **General:** XML-based modular command configuration, grouping, if-then-else, dynamic parameters, user input, etc. Or alternatively simple use PowerShell Cmdlets.
* **Dataverse/Dynamics CE:**
    * Regular authentication but also exotic scenarios (e.g. Application User with Client ID)
    * Typical deployment tasks (import and export of solution, publish, ...)
    * Advanced deployment tasks (set root business unit, language activation, update system and personal settings, activate and deactivate processes and views, deactivation of sample data, ...)
    * Export (!) and import of plug-in registrations
    * Data management tasks (assignment of entities, deletion of entity records and entities, Fetch XML execution, export of audit logs to HTML, retrieve generic entity information, rendering of email templates, ...)
    * Quality assurance (check for activated processes and plug-in steps,  
* **Scribe Online:**
    * Export and import of Scribe Solutions, Maps and Lookup Tables
    * Replacement of connections
    * Map validation
    * Run Scribe solutions
    * Scheduling and multi-threading of Scribe solutions (high-performance imports)
* **Windows:** File processing, external program execution, update registry, Windows Service management, ...
* **SQL:** Statement execution
* **Other:** User input, logging, send email, string manipulation, ...
