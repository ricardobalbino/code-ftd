# Execute as Admin

BCA plugins are registered and executed in the context of the initiating user. In most situations this is the intended behavior, but there are some special cases, where you need to execute the whole plugin, or parts of it, as an admin user (System user).

## Whole plugin as admin
To elevate a whole plugin execution including it's invoked services as an admin simply set the `AsAdmin` property on the `PluginRegistration` class attribute above the plugin.

```csharp
    [PluginRegistration(
        EntityLogicalName = Account.EntityLogicalName,
        MessageName = MessageNames.Create,
        Stage = Stage.PostOperation,
        <b>AsAdmin = true</b>
    )]
```

Please bear in mind that the plugin step itself gets registered still registered to execute under the initiation user. The elevation solely happens inside of the plugin not outside of it.

## Queries as admin

The BizApps Core Accelerator also allows you to only execute specific CRUD operations as admin. In the following example this is demonstrated using the `AccountContactSyncService`:

```csharp
public class AccountContactSyncService : IAccountContactSyncService
{
    private readonly IContactRepository _contactRepository;
    private readonly IRepository<Account> _accountRepository;

    public AccountContactSyncService(IContactRepository contactRepository, IRepository<Account> accountRepository)
    {
        _contactRepository = contactRepository;
        _accountRepository = accountRepository;
    }
}
```

As you can see, this service requires two different repositories. These will be injected by the [Dependency Injection Container](Dependency-Injection.md) and will be run in the context of the initiating user. To elevate these repositories you simply need to call the `AsAdmin<T>()` method. This returns a **new instance** of the repository that uses the admin context:

```csharp
public class AccountContactSyncService : IAccountContactSyncService
{
    private readonly IContactRepository _contactAdminRepository;
    private readonly IContactRepository _contactRepository;
    private readonly IRepository<Account> _accountRepository;

    public AccountContactSyncService(IContactRepository contactRepository, IRepository<Account> accountRepository)
    {
        _contactRepository = contactRepository;
        _contactAdminRepository = contactRepository.AsAdmin<IContactRepository>();
        _accountRepository = accountRepository;
    }
}
```

This can even be drilled down to a per query elevation by simply elevating certain parts of the business logic to use an elevated context.

```csharp
public class AccountContactSyncService : IAccountContactSyncService
{
    ...

    public void DoSomething()
    {
        var specialContacts = contactRepository.AsAdmin<IContactRepository>().GetAllSpecialContacts();
        ....
    }
}
```

## Dataverse requests as admin
If special Dataverse requests need to be elevated you can leverage the ICrmMessageService. It has the same semantics as the repository.

```csharp
public class MyService : IMyService 
{
    public MyService(ICrmMessageService messageService)
    {
         _messageService = messageService;
    }

    public void DoSomething()
    {
        ...
        var response = _messageService.AsAdmin().ExecuteRequest<QualifyLeadResponse>(request);
        ...
    }
}
```