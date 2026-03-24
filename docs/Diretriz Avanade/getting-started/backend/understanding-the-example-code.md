# Understanding the Example Code

The engagement you setup through the **BizApps Platform** contains examples to help you get started as quickly as possible. This section guides you through the different examples and helps you understand them.

## Included Examples

The included examples can be grouped into different categories. Below is a list of those categories and a brief explanation of all related examples.

### Backend Plugins

To help you understand the structure, multiple Dataverse plugins are included that follow the best practices of the **BizApps Core Accelerator**. 

| Plugin Name                   | Description                                                                                     |
| ----------------------------- | ----------------------------------------------------------------------------------------------- |
| AccountCreatePreOpsPlugin     | A plugin that prefills certain values when creating a new account.                              |
| AccountCreatePostOpsPlugin    | A plugin which creates a related contact (if needed).                                           |
| AccountUpdatePlugin           | A plugin that syncs data from an account to a contact.                                          |
| AccountDeletePlugin           | An example of how to handle special Dataverse events. In this case the `DeleteRequest`       |
| AccountRetrieveMultiplePlugin | Another example on how to handle special Dataverse events, in this case a `RetrieveMultiple` |
| MyCustomApiPlugin             | Example implementation of a custom api plugin                                                   |

### Commands

An example implementation can be found in the `UnsubscribeCommand` class.

### Custom APIs

An example implementation for a custom api aware plugin can be found in the `MyCustomAPI` class.
