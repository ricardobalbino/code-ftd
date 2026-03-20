# Examples

## Mocking repositories

The repository interface:
```csharp
public interface IAccountRepository : IRepository<Account>
{
    List<Account> GetAllAccountsOfSalesArea();
}
```

The service:
```csharp
public class MyService
{
    private readonly IAccountRepository _accountRepository;

    public MyService(IAccountRepository accountRepository)
    {
        _accountRepository = accountRepository;
    }

    public void Execute(Account account)
    {
        var accounts = _accountRepository.GetAllAccountsOfSalesArea(account.SalesArea);
        foreach(var acc in account)
        {
            ...
            _accountRepository.Update(acc);
        }
    }
}
```

The test class:
```csharp
[TestClass]
public class MyServiceTest
{
    private IAccountRepository _accountRepository;
    private MyService _service;

    [TestInitialize]
    public void SetUp()
    {
        _accountRepository = Substitute.For<IAccountRepository>();
        _service = new MyService(_accountRepository);
    }

    [TestMethod]
    public void ReasonableTestMethodName()
    {
        var accounts = new List<Account>() 
        {
            new Account() { },
            new Account() { },
            new Account() { }
        };

        _accountRepository.GetAllAccountsOfSalesArea(Arg.Any<int>()).Returns(accounts);

        _service.Execute();

        _accountRepository.Received(accounts.Count()).Update(Arg.Any<Account>());
    }
}
```

So based off the AAA pattern we're arranging the test data at the beginning of the test method:
```csharp
        var accounts = new List<Account>() 
        {
            new Account() { },
            new Account() { },
            new Account() { }
        };

        _accountRepository.GetAllAccountsOfSalesArea(Arg.Any<int>()).Returns(accounts);
```

Since the repository's method returns a list of accounts we're creating a list of accounts and instructing the method that it return for any argument the list of accounts:

```csharp
        _accountRepository.GetAllAccountsOfSalesArea(Arg.Any<int>()).Returns(accounts);
```

With that when calling the ```_service.Execute()``` method it will receive the mocked list of accounts from above.

To validate that the given service class logic was executed properly we can use another helper method from NSubstitute called ```.Received(...)``` to validate how many times a given method was called. In this case we just validate that the method was called the same amount of times as entries in the accounts list.

## Mocking AttributeChangedService

The service:
```csharp
public class MyService
{
    private IAttributeChangedService<Account> _changedService;

    public MyService(IAttributeChangedService<Account> changedService)
    {
        _changedService = changedService;
    }

    public bool Execute(Account account)
    {
        if (_changedService.HasChanged(a => a.SalesArea))
            return true;
        else
            return false;
    }
}
```

The test class:
```csharp
[TestClass]
public class MyServiceTest
{
    private IAttributeChangedService _changedService;
    private MyService _service;

    [TestInitialize]
    public void SetUp()
    {
        _preImage = new Account();
        _target = new Account();
        _changedService = new AttributeChangedService(_preImage, _target);

        _service = new MyService(_changedService);
    }

    [TestMethod]
    public void DoesSomething_WhenSalesAreaHasChanged()
    {
        _preImage.SalesArea = 0;
        _target.SalesArea = 1;

        var result = _service.Execute(_target);

        result.ShouldBe(true);
    }
}
```

