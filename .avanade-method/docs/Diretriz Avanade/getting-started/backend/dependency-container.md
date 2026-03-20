# Dependency Container

One essential part of the **BizApps Core Accelerator** is the Dependency Container. It is used to resolve and handle all dependencies within your backend code. This guide just covers the basics and everything you need to know right now to start developing plugins. For detailed information about how everything works, please head over into the [Development Guide](../../development/Backend/Dependency-Injection.md).

## How to use the Dependency Container

As shown in the last section, all plugins get a reference to the `IDependencyContainer` passed as a parameter to their `Execute` method. It behaves similar to the `IServiceProvider` you usually get from the Dataverse SDK and in fact, you can use it to get the same services (e.g. `IExecutionContext` or `ITracingService`). However, in addition to that, you can use it to resolve all classes within your project. Let's see how that looks in the code:

```csharp
public override void Execute(IDependencyContainer container)
{
    // You can use it to get the regular Dataverse SDK types
    var executionContext = container.Resolve<IExecutionContext>();
    var tracingService = container.Resolve<ITracingService>();

    // But also to get your custom services and repositories
    var syncService = container.Resolve<IAccountSyncService>();
    var contactRepo = container.Resolve<IContactRepository>();
}
```

## How to Register Services

The simple answer: You usually don't! While this makes it easy to use, it is still essential to understand how it works internally.

Fortunately, it is quite easy to understand how the classes are registerd. By default, the framework searches for all public classes and registers each to its default interface (if found). Default interface means that the interface is named exactly like your class, just with a capital `I` before that.

=== "Working Example"
    ```csharp
    public interface IExampleService 
    {

    }

    public class ExampleService : IExampleService 
    {

    }
    ```
=== "Common Mistake #1"
    ```csharp
    // This won't work, as the naming of the interface and the class doesn't match.

    public interface IMyService 
    {

    }

    public class ExampleService : IMyService 
    {

    }
    ```
=== "Common Mistake #2"
    ```csharp
    // This won't work, as the casing of "ExampleService" is different between the interface and the class.

    public interface IExampleservice 
    {

    }

    public class ExampleService : IExampleservice 
    {

    }
    ```

All those services are registered with the lifetime `Transient`. This means, each call to `#!csharp container.Resolve<IExampleService>()` will return a new instance of that `ExampleService`.