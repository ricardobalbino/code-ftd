# State and Target Awareness

In Dataverse the `InputParameters` plays a vital role and additionally have an influence on the software itself. To mitigate this we came up with a fairly simple approach which abstracts this particular problem away so the rest of the domain implementation does not have to care about it.

## Why we need an abstraction
Because our `Operation`s are modular and are doing only one thing they can be reused/registered for different combinations of messages, stages and even entities.

The context of a plugin revolves around three major parameters:
1. The stage (10..40) (Pre-Validation..Post-Operation)
2. The message (Create, Delete, etc.)
2. The target

So for instance when working with the `target` in stage 20 you should not create an `UpdateRequest`.

## The IPluginExecution is dead long live the OperationContext

The `OperationContext` is the replacement for the `IPluginExecutionContext` providing to some extend the same methods. Additionally it provides save guard methods for the problems described above. These are for instance:
- Update
- Delete
- Associate
- Disassociate

## How comes the OperationContext into play
Picking up the same example from above: Updating the target. Imagine we're changing, prefilling some attribute of an account and do want to persist those.

> Do not take this example as a best practice. You should extract business logic into a services see chapter [here](Plugins/Write-a-Plugin.md)

```csharp
public void FooOperation : IOperation<Account>
{
    public FooOperation(IRepository<account> accountRepository)
    {
        _accountRepository = accountRepository;
    }

    public void Execute(Account target)
    {
        target.MyAttribute = "dummy";

        _accountRepository.Update(target);
    }
}
```

With the basic example above we can have a look into the update method of the repository.

```csharp
public void Update(T entity)
{
    try
    {
        _context.UpdateEntity(entity);
    }
    catch (Exception ex)
    {

        _logger.TraceError(new TypeBoundCategory(GetType()), "Unable to update entity " + entity.LogicalName, ex);
        throw new InvalidPluginExecutionException($"Error while updating entity {entity.LogicalName} with id {entity.Id}. Inner Exception: {ex}", ex);
    }
}
```

As you can see there's nothing much what's happening there. It just delegates the call to the `IOperationContext` and does proper exception handling.
Now depending on your **stage** and **message** the repository get's different implementations of the `IOperationContext` injected. This can e.g. be an `PreOperationContext`, an `PostOperationContext` or a `DeleteOperationContext`. 

If we register our `FooOperation` in the Pre-Operation then the `PreOperationContext` will be injected. This one knows, that it should not update the target using an `UpdateRequest`, as this would result in an endless loop, but instead update the real target instance. On the other hand, if we register it in the Post-Operation we need to send out a real `UpdateRequest`, as we are after the Main-Operation and our changes would not get persisted otherwise. Luckily the BizApps Core Accelerator is smart enough to inject the appropriate `PostOperationContext`, which does exactly that!
