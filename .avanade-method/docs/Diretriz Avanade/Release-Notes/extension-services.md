# Common Business Requirements Boosted by Extension Services

## Summary

Backend core has been extended with Extension Services which solve typical and partly very advanced business logic challenges. Developers can simply utilize the services by calling the public methods from plug-ins or client applications and instantly save a lot of efforts and increase quality by leveraging proven logic.

## Benefits

|Extension Service| Challenges Solved | Build Hours Saved
|--|--|--|
| FetchXmlService | <ul><li>Define flexible, Fetch XML-based rules which can be rapidly validated against Dataverse records (in-memory)</li><li>Support for all kinds of versatile use cases where rules have to be evaluated</li></ul> | 200
| EmailService | <ul><li>Send emails effectively from the backend</li><li>Render Email Templates which can be used for all kinds of use cases outside plain email handling (for example push or in-app notifications)</li></ul> | 80-100
| DynamicExpressionService | <ul><li>Resolve and format textual expressions to Dataverse column values without running into OOTB limitations (for example Merge Fields)</li></ul> | 200-250
| AzureServiceBusService | <ul><li>Rapidly send well-defined messages to Service Bus or use in Webhook (message properties linked to Dataverse record and related record column values)</li></ul> | 80-120
| AzureActiveDirectoryTokenService | <ul><li>Procure and cache OAuth token to access an Azure resource from the backend</li></ul> | 20
| SecurityRuleService | <ul><li>Full-fledged engine to assign owner (or set owning business unit) of Dataverse records based on Fetch XML-based rules or parent record</li></ul> | 80-120
| MetadataService | <ul><li>Rapid access to Dataverse metadata in the backend (smart caching)</li></ul> | 40
| TimezoneService | <ul><li>Effective time conversion from local to UTC and vice versa</li><li>Use customer-related Timezones independently from user settings</li><li>Cached Timezone information</li></ul> | 40-80| TranslationService | <ul><li></li></ul> |
| TranslationService| <ul><li>Utilization of .resx files for both backend and frontend</li><li>Cached translation messages</li></ul> | 40-60| TranslationService | <ul><li></li></ul> |