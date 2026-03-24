# Writing JavaScript Tests

This section provides details on how to write basic frontend unit tests, mock form attribute interaction and provide faked async operation responses.

## Getting Started

In the *Web* project (`example-project.code-workspace`) there's a `tests` folder, which contains some sample unit tests. The folder structure shall be the same as in the `src` folder. E.g. when writing a test for `src/forms/Account/Form/account.form.controller.ts` you shall place the test code in `tests/forms/Account/Form/account.form.controller.test.ts`.

### Test Structure

Let's start with the basic structure of javascript unit tests:
```ts
import "should";

describe("Account Controller Tests", function () {
    test("My First Test", async function() {
        
    })
});
```

The `describe` function is used to group the tests into components. E.g. all tests, that target the account form controller are grouped into the "Account Controller Tests" component. With the `test` function you define your actual test code. They are normally grouped under a component (*describe*), but don't have to be.
> Note: When declaring a test function with `test`, make sure to use the `async` keyword, as it's required most of the time!

### DynamicsContextMock

What you'll need in nearly every test is the `DynamicsContextMock`. It's a mock-implementation of the `DynamicsContext`, which holds the Dataverse execution context, form context and global context.
There are different ways of setting up a `DynamicsContextMock`:

1. Create the mock using a [Generated Form Object](../Early-Bound-Forms.md) and it will automatically set-up a mock attribute for each attribute on the form. E.g. 
`dynamicsContext = DynamicsContextMock.createFromForm(AccountForm);`
2. Create any empty one, without any pre-defined attributes with 
`dynamicsContext = new DynamicsContextMock(AccountForm);`
3. Create one with custom data
```ts
const mockData = new DynamicsContextMockData({
    attributes: [
        new AttributeMock({
            name: "test-attribute",
            value: "initial value",
        })
    ]
});
const dynamicsContext = new DynamicsContextMock(mockData);
```

After creating the `DynamicsContextMock` you can use it just like the normal `DynamicsContext`. E.g. create a form object to easily modify values:
```ts
const dynamicsContext = DynamicsContextMock.createFromForm(AccountFormObject);
const form = new AccountFormObject(dynamicsContext.formContext);

// Set value for PrimaryContactId on the mock
form.PrimaryContactIdAttribute.setValue(Guids.GuidA); 

// Read value of the mock object. Often used to make assertions in the tests.
form.PrimaryContactIdAttribute.getLookupId().should.equal(Guids.GuidA); 
```

### Fake Server

The `FakeServer` is used to mock Web API calls to the Dataverse OData api. For obvious reasons we cannot execute real web-api calls to the Dataverse environment from within our unit tests. That's where the `FakeServer` comes in handy, as it mocks the web-api calls and returns pre-defined responses. Perfect for unit tests!
For this to work you need to set-up the fake server and register the fake responses in advance. The following code example shows this process:
```ts
const fakeServer = new FakeServer();
fakeServer.registerResponse(
    OdataRegExBuilder.retrieveSingle(Contact, Guids.GuidA),
    new FakeODataResponse({firstname: "Simon", lastname: "Bauer"}));

// ... Your Test Code ...

fakeServer.restore();
```
1. Create the FakeServer instance
2. Register a fake response. For this you need two parameters
   1. A regex expression which matches the URL of the odata call. You can use the helper class `OdataRegExBuilder` to create regex expressions like shown above
   2. The response that will be returned. Can either be a `Response` or `FakeODataResponse`
3. Clean Up! You need to call the "restore" function on the `FakeServer`

#### Mocking OData Commands
Mocking a command is also possible with the `FakeServer`. The syntax is a bit different tho:
```ts
// 1. Param: CommandName, 2. Param: Command Result, 3. Param: Command Parameters
const testCommandResponse = FakeCommandResponse("test-command", { success: true }, {  });

// Register FakeCommandResponse. Make sure to use the [FakeCommandResponse].matcher function instead of a regex expression.
fakeServer.registerResponse(testCommandResponse.matcher, testCommandResponse);
```


### Spies

In the BizApps Core Accelerator we use [**sinon.js**](https://sinonjs.org/) for mocking and spying on existing objects. While you can use any spying framework you want to, we highly recommend **sinon**, as it provides a good set of features, while still being easy to use.

Spying on an existing object is easy. The following code sample illustrates the process:
```ts
// We want to spy on the function "getData" of the "SampleModel"
const sampleModel = new SampleModel();

// This is the important line, that installs the spy on the getData function
const sampleModelGetDataSpy = sinon.spy(sampleModel, "getData");

const sampleData = sampleModel.getData();

// Check with the spy if the function getData has been called
contactModelSpy.calledOnce.should.be.true();
```

For more information about spies and sinon in general you can check out the [documentation](https://sinonjs.org/releases/v7.3.2/).

### Fluent Rule Testing

Testing [FluentRules](../Fluent-Rules.md) is also quite easy with the helper classes we provide. It works like the following
1. Create the `DynamicsContextMock` using the correct [Early Bound Form](../Early-Bound-Forms.md)
2. Create an instance of the FormController you want to test. (e.g. `new AccountFormController(dynamicsContextMock)` and initialize it
3. Setup Fluent Rule Verifiers
4. Execute all FormVerifierVerifiers

A `FormVerifier` is nothing else than a set of two function:
- One to perform the action on the form. This is used to programmatically perform the action, that would normally done by a user on the form. E.g. enter some value in the "FirstName" field.
- The second function is called shortly after the first one, to verify that the fluent rules correctly changed the form details.

To fully understand this it is quite helpful too look at an example:
Let's imagine we have a FluentRule that changes the visibility of the CreditLimitAttribute based on whether a PrimaryContactId is set on the current Account Form or not. To test this we need to do two things:
- Set the PrimaryContactId to null and verify that the CreditLimitAttribute is not visible
- Set the PrimaryContactId to a valid GUID and verify that the CreditLimitAttribute is visible

Each of these two test scenarios gets it's own `FormVerifier`:
```ts
const verifiers = [
    // Verify CreditLimit Visibility (Not Visible)
    new FormVerifier(() => {
            form.PrimaryContactIdAttribute.setValue(null);
        }, () => {
            form.CreditLimitAttribute.getVisible().should.be.false();
        }
    ),

    // Verify CreditLimit Visibility  (Visible)
    new FormVerifier(() => {
            form.PrimaryContactIdAttribute.setValue(Guids.GuidA);
        }, () => {
            form.CreditLimitAttribute.getVisible().should.be.true();
        }
    ),
];
```

To execute these verifiers and check that all assertions are correct we need to call the `FormVerifier.executeVerifiers(...)` method. This expects an array of all the verifiers we want to execute as the first parameters. So with the example above we just call:
```ts
await FormVerifier.executeVerifiers(verifiers);
```

To get things straight the following code is a full example:

```ts
describe("AccountFluentRules", function () {
    test("Executes Correctly", async function () {
        const dynamicsContext = DynamicsContextMock.createFromForm(AccountFormObject);
        const form = new AccountFormObject(dynamicsContext.formContext);
        const formController = new AccountFormController(dynamicsContext);
        formController.init();

        const verifiers = [
            // Verify CreditLimit Visibility (Not Visible)
            new FormVerifier(() => {
                    form.PrimaryContactIdAttribute.setValue(null);
                }, () => {
                    form.CreditLimitAttribute.getVisible().should.be.false();
                }
            ),

            // Verify CreditLimit Visibility  (Visible)
            new FormVerifier(() => {
                    form.PrimaryContactIdAttribute.setValue(Guids.GuidA);
                }, () => {
                    form.CreditLimitAttribute.getVisible().should.be.true();
                }
            ),
        ];

        await FormVerifier.executeVerifiers(verifiers);
    });
});
```

