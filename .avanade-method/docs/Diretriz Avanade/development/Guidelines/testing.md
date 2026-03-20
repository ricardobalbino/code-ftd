# Testing

### Test Principles

Generally, the shift left principle is applied which means that logic is tested as early as possible, preferably in fast running unit tests without the need of testing core logical functions with longer running integration or UI automation tests.

The creation of tests for all delivered pieces of code allows for consistent, high quality delivery. Developers are required to author the tests for each piece of code they deliver. These tests can be run during builds allowing the team to catch any inconsistencies before code being deployed.

### Test Categories

This implies the following main test categories:

- **Unit Tests**

  Frontend and backend code which can be tested fast in Visual Studio (Code) or in the Azure DevOps pipeline without the need of having an Azure or Dynamics environment to run the tests against. A mock framework is used to simplify the test of typical CRUD operations.

- **Integration Tests**

  Backend code tests which concentrate on API endpoints and cross-component interactions. This can be for example the test of a Logic App endpoint which verifies that the Logic App can process an emulated message and very the outcome in a queue.

- **UI Automation Tests**

  Frontend functionality tests which can be run with a live browser opening during the test execution or also automatically in an Azure DevOps pipeline. These tests must concentrate on realistic user scenarios (for example create a case by opening a form, entering the data and save the record).

### Test Projects

Each solution project has a related Test project where the tests must be
specified.

### Test Naming

Tests must be named according to the following schema:

`[<Class>]_<Method>_Should_<ExpectedBehavior>_When_<StateUnderTest>`

_Example:_

`ContactService_AssociateContactWithAccount_Should_AssociateSingleContactWithAccount_When_ContactLookupIsSetInCase`

### Test Data Definition

For building automated (integration) tests, it is sometimes required to
prepare/create records in Dataverse (e.g. create a Case to attach activities
via your new code or doing validations). Here only the core records
should be created in the test to avoid too much overhead in the test
itself.

Refer to [Test Data](../../getting-started/next-steps/data-management.md#test-data) for further information on test data.
