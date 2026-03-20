# CRUD Operations

> CRUD stands for **C**reate, **R**ead, **U**pdate, and **D**elete of records

In a data centric business application like Dataverse everything revolves around creating, reading, updating and deleting data. This is typically achieved by using the methods of the `OrganizationService`. To achieve a clear structure in your application you should utilize any CRUD operation using the below described pattern.

## Why another abstraction?

Here's a quote from Martin Fowlers book called "Catalog of Patterns of Enterprise Application Architecture" about the [Repository pattern](https://martinfowler.com/eaaCatalog/repository.html)
>A system with a complex domain model often benefits from a layer, such as the one provided by Data Mapper (165), that isolates domain objects from details of the database access code. In such systems it can be worthwhile to build another layer of abstraction over the mapping layer where query construction code is concentrated. This becomes more important when there are a large number of domain classes or heavy querying. In these cases particularly, adding this layer helps minimize duplicate query logic.

>A Repository mediates between the domain and data mapping layers, acting like an in-memory domain object collection. Client objects construct query specifications declaratively and submit them to Repository for satisfaction. Objects can be added to and removed from the Repository, as they can from a simple collection of objects, and the mapping code encapsulated by the Repository will carry out the appropriate operations behind the scenes. Conceptually, a Repository encapsulates the set of objects persisted in a data store and the operations performed over them, providing a more object-oriented view of the persistence layer. Repository also supports the objective of achieving a clean separation and one-way dependency between the domain and data mapping layers.

With this explanation it is obvious to add another layer to distinctively separate the core domain from any data operations. Not just because we want to following the [SRP](https://en.wikipedia.org/wiki/Single_responsibility_principle) but because we want to have clear separated concerns, better testability and a clear structure in our code base.

## Repository pattern applied

As described we have to define a generic layer which acts as the data layer which can be integrated using dependency injection into our core domain. This looks like such in the BizApps Core Accelerator:

```csharp
    public interface IRepository<T> where T : EntityDTO, new()
    {
        IRepository<T> AsAdmin<TRepository>() where TRepository : IRepository<T>;
        T Create(T entity);
        void Delete(T entity);
        void Update(T entity);
        bool Exists(Guid id);
        IQueryable<T> GetAll();
        T GetById(Guid id);
    }
```

With the generic interface you can inject any repository into any class for every generated early-bound entity. Look at the following example:

```csharp
    public class Foo
    {
        public Foo(IRepository<Account> accountRepository, IRepository<Contact> contactRepository)
        {
            _accountRepository = accountRepository;
            _contactRepository = contactRepository;
        }

        public void DoSomething(Guid accountId, Guid contactId)
        {
            var account = _accountRepository.GetById(accountId);
            var contact = _contactRepository.GetById(contactId);
            ...
        }
    }
```

## Writing custom queries
At some time in the project you have to write a more complex query to aggregate you need for the business requirement. The `IRepository<>` interface provides a `GetAll()` method which provides a queryable which you can write your LinQ query with.

>Reminder: You should not write any data query inside of your business domain

This query has to be implemented into a distinct repository class like an `AccountRepository`. With dependency injection in place you can create your own repository class which hold any query for this particular entity. 

First you have to define the interface which provides the query methods:

```csharp
public interface IAccountRepository : IRepository<Account>
{
    public IEnumerable<Account> GetAccountsForSalesRegion(SalesRegion salesRegion);
    public IEnumerable<Account> GetAccountsOfPrimaryContact(Guid contactId);
}
```
With the interface in place which outlines the available methods we can now create the implementation like this:

```csharp
public class AccountRepository : Repository<Account>, IAccountRepository
{
    public IEnumerable<Account> GetAccountsForSalesRegion(SalesRegion salesRegion)
    {
        GetAll()
                .Where(a => a.SalesRegionId == salesRegion.Id && a.Status == Active)
                .Select(a => new Account { Id = a.Id, Name = a.Name, Revenue = a.Revenue });
    }

    public IEnumerable<Account> GetAccountsOfPrimaryContact(Guid contactId)
    {
        GetAll()
                .Where(a => a.PrimaryContactId == contactId && a.Status == Active)
                .Select(a => new Account { Id = a.Id, Name = a.Name });
    }
}
```
As you can see you have to derive from `Repository<>` and the specific interface `IAccountRepository`. The base class `Repository<>` contains the infrastructure to utilize the CRUD operations.

Based on the principles learned using dependency injection you can now injection the `IAccountRepository` in any class where you need to utilize the above queries.

### Join statements

The Framework provides an easy way to write entity based join statements. Because of the weird linq 2 entities syntax we've only provided a linq2sql extensions. You can write join statements like this:

```csharp
    return (
        from c in GetAll()
        join account in Join<Account>() on c.AccountId.Id equals account.Id
        select c
    );
```

The join statement only works with generated entities so TypedReferences won't work.

## Updating target in stage 10 (Pre-Validation) or 20 (Pre-Operation)
When executing an operation utilizing a repository which tries to update the current `target` provided by the InputParameters, the repository is aware of that fact. We achieve this by using another class called `OperationContext`. You can read about its logic/purpose [here](Stage-and-Target-awareness.md).

When this `OperationContext` encounters that you're trying to update the current `target` it will not execute an `UpdateRequest` it will update the current `target`'s attributes so the changes to the record will be brought into the main operation stage (30).

## Updating non target records
When updating a record which is not the target of a plugin execution you have to make sure to isolate the changes made to that record to minimize plugins/flows of being triggered. You're achieving this by creating a new instance of that entity and assign only the changes necessary, e.g.:

```csharp
   var existingContact = _contactRepository.GetById(account.PrimaryContactId.Id);

   var contact = new Contact {
       Id = existingContact.Id,
       AttributeWhichNeedsAnUpdate = "New Value"
   }

   _contactRepository.Update(contact);
```

We're currently investigating the possible application of the memento pattern which would provide us the capabilities to reuse the same retrieved record to apply the changes and to use as well to update it. We'll update this documentation when this feature is available.