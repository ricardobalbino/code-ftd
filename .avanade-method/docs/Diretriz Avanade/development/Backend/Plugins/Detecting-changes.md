# Detecting Change

We've described how we [handle the target](Handling-the-target.md). Because of the merged target (pre-image + target) detecting change is utilized using the `AttributeChangedService`.

## How it detects change
The `AttributeChangedService` gets the plugin execution context injected as a dependency. Using the execution context we're having access to the `target` as well as to the `pre image`. With these we're able to compare the two and detect change.

## How to get change detection
The `AttributeChangedService` has a type dependency to the class `Entity` which is being used for the generated entities. You can pass in any generated entity but you only get valid results when the type matches the plugin registration.

If for instance you're injecting a `AttributeChangedService<Account>` into a service which is being used in a contact pipeline the `AttributeChangedService<Account>` won't fail but returns default values when calling it's methods since the target is not of type account but contact.

You can inject a `AttributeChangedService<T>` like this:

```csharp
    public class FooService : IFooService
    {
        public FooService(IAttributeChangedService<Account> changedService)
        {
            _changedService = changedService;
        }
    }
```

## How to detect change

The `AttributedChangedService` has 4 distinct methods:

- HasChanged(..)
- HasChangedTo(..)
- HasChangedFrom(..)
- GetPreviousValue(..)

Every method requires you to pass a delegate `Func<TOut, TIn>`. Every attribute type is utilized and can be evaluated. See this example below:

```csharp
    _changedService.HasChanged(a => a.Name);
    _changedService.HasChanged(a => a.Revenue);
    _changedService.HasChangedFrom(a =>a.Name, "My fictional company name");
    _changedService.HasChangedTo(a => a.Name, "My fictional company name");

    var previousAccountName = _changedService.GetPreviousValue(a => a.Name);
```

# Mocking the AttributeChangedService

The attribute changed service receives a delegate when it gets invoked (e.g. _changedService.HasChanged(**a => a.Name**)) which can't be mocked like you would usually mock an argument with Arg.Is<>() or Arg.Any<>().

When an attribute changed service is used in any business logic you can use its special constructor to create a real pre image and the target (which will be already existing in your tests since your executing logic on a given entity). The code looks like this:

```cs 
[TestInitialize]
public void SetUp() 
{
    _preImage = new Account();
    _target = new Account();

    _changedService = new AttributeChangedService<Account>(_preImage, _target);
    _myService = new MyService(_changedService);
}

[TestMethod]
public void MyTest()
{
    _preImage.Name = "ABC";
    _target.Name = "DEF";

    _myService.Execute(_target).ShouldBe(true);
}
```