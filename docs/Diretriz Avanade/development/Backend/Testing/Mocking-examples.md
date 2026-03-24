# Mocking Examples

## How to mock an interface

```csharp
var accountRepository = Substitute.For<IRepository<Account>>();
```

## How to mock return values from mocked objects

```csharp
var accountRepository = Substitute.For<IRepository<Account>>();

var accounts = new List<Account>() 
{
    new Account(),
    new Account(),
    ...
}
accountRepository.GetAllAccountsFromSalesArea(Arg.Any<int>).Returns(accounts);
```

## How to mock methods for specific arguments

Use Arg.Is(...) instead of Arg.Any<>()

```csharp
var accountRepository = Substitute.For<IRepository<Account>>();

var accountsOfSalesArea1 = new List<Account>() 
{
    new Account(),
    new Account(),
    ...
}
accountRepository.GetAllAccountsFromSalesArea(Arg.Is(1)).Returns(accounts);

var accountsOfSalesArea2 = new List<Account>() 
{
    new Account(),
    new Account(),
    ...
}
accountRepository.GetAllAccountsFromSalesArea(Arg.Is(2)).Returns(accounts);

```
## Checking received calls

### Exactly one
```csharp
_accountRepository.Received().Update(Arg.Any<Account>());
```
### Specific amount
```csharp
_accountRepository.Received(5).Update(Arg.Any<Account>());
```

### With specific arguments

```csharp
_accountRepository.Received().Update(Arg.Is<Account>(a => a.SalesArea == 1));
```

## Return from a function

Using and adjusting the value passed into a mocked method
```csharp
_accountRepository.Create(Arg.Any<Account>()).Returns(info =>
{
    var acc = info.Arg<Account>();
    acc.Id = Guids.A;
    return acc;
});
```
