# Entity Management Cockpit (EMC)

The tool eases the task to import data into CRM in a very light-weight way, overcoming the deficiencies of a standard import and with advanced possibilities for automation. It allows users to define certain data sets in Excel and then publish the data into PowerApps/Dynamics CE via a simple Windows form UI or as a command-line operation which minimizes the number of manual steps.

The tool mainly addresses scenarios where configuration data for test or rollout purposes has to be replayed and/or distributed quickly into multiple target environments. This is for example helpful if you want to provide the same set of test data into a multitude of environments and PowerApps/Dynamics organizations.

The target audience comprises developers, testers, quality specialists, rollout coordinators, business analysts and many more.

# What's new?

Release 3.0.2:

* Corrected import of single .csv files

Release 3.0:

* Removed `FakeXrmEasy` to be able to continuously support .NET Core as the package will be moving to a paid license model with the new versions which support .NET Core.
* Added `UseCaseSensitiveKeys` setting so that when looking up records, case-sensitive comparison can be applied as an opt-in. 

Release 1.7.1:

* Added support for export of Currency (Money) fields.

Release 1.7.0:

* Added `ignoreselfreferencing` attribute to speed up creation or updates of entities which are self-referencing.
* Enabled `"title"` override attribute to be considered for all column exports.
* Enabled columns with concatenated values (from other columns).
* Restructured status handling logic to remove SetStateRequest and replace with regular update.
* Added feature to close an opportunity as won or lost.
* Improved handling of Two Option values by looking at the localized label instead of "Yes/No".
* Fixed export issue which occurred with some columns due to AliasedValue type.
* Added support for Choices fields (Multiple Option Sets) for import and export.
* Enabled export and import to and from a folder if it is provided as the file location instead of a dedicated file.

Release 1.6.0:

* Improved error handling for wrong XML configurations.
* Moved Quick Reference guide (Word) to this markdown (this README).
* Improved logging for Security Role assignments.
* Added possibility to determine Security Role names by column header rather than the optional "typeextension" attribute.
* Removed obsolete "globaloptionset" configuration
* Added automated column type detection
* Added possibility to use proxy service when performing the connection to Dynamics/Dataverse.
* Defaulted timezone to null to avoid date time conversion during CSV export.
* Added date formatting possibility for date time field during CSV export.
* Fixed foreign key issue with casing of Guids to avoid that upper case and lower case need to match

# Feature Highlights

* Quick, direct and optionally automated integration of Excel, CSV and XML data into Dataverse
* Windows Forms and Command-Line functionality
* Create, Update, Delete of records (Table based) incl. all attribute types
* Export/Import of N:M Relations
* Import of users and teams including association of team memberships, field security profiles and security roles
* Combined lookup keys to identify existing records for updates
* Population of entity fields with default values or current timestamp
* Create and update by Guid which allows for a 1:1 migration from one source system to another preserving relationship
* Bulk Activation/Deactivation of records (also system user)
* Status Reason Update (incl. Resolve Case and Close Opportunity)
* Attachment (Annotation) Import
* Email Template Creation
* Export of Advanced Find queries
* Creation/update and scheduling of Bulk Deletion jobs
* Import and export of file attachments
* Import and export of Sales Literature
* Import and export of Document Templates
* Dynamic Expressions (resolve Lookups during import)
* Import Duplicate Detection Rules
* Bulk merge of Table rows
* Import Calendar data
* Resolve Case
* Run as Azure WebJob
* Impersonation of tool actions
