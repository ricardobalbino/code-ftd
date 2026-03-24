# Services / Business Logic

A service encapsulates the specifc business logic for a given use case. They act as coordinators, allowing higher level functionality between many different smaller parts. These would include things like OrderProcessor, ProductFinder, FundsTransferService, and so on. 

Since Domain Services are first-class citizens of our domain model, their names and usages should be part of the Ubiquitous Language.  Meanings and responsibilities should make sense to the stakeholders or domain experts.

Here is an example:

```csharp
public interface IAccountPrefillService
{
    void Prefill(Account account);
}
```

```csharp
public class AccountPrefillService: IAccountPrefillService
{
    private readonly IRepository<Account> _repository;

    public AccountPrefillService(IRepository<Account> repository)
    {
        _repository = repository;
    }

    public void Prefill(Account account)
    {
        account.Name += " (Prefilled)"
        _repository.Update(account);
    }
}
```

An application service has an important and distinguishing role - it provides a hosting environment for the execution of domain logic. As such, it is a convenient point to inject various gateways such as a repository or wrappers for external services.

In addition to being a host, the purpose of an application service is to expose the functionality of the domain to other application layers as an API. This attributes an encapsulating role to the service - the service is an instance of the [facade pattern](https://en.wikipedia.org/wiki/Facade_pattern). Exposing objects directly can be cumbersome and lead to [leaky abstractions](https://en.wikipedia.org/wiki/Leaky_abstraction) especially if interactions are distributed in nature. In this way, an application service also fulfills a translation role - that of translating between external commands and the underlying domain object model. The importance of this translation must not be neglected. 

For example, a human requested command can be something like “transfer $5 from account A to account B”. There are a number of steps required for a computer to fulfill that command and we would never expect a human to issue a more specific command such as “load an account entity with id A from account repository, load an account entity with id B from account repository, call the debit method on the account A entity…”. This is a job best suited for an application service.
