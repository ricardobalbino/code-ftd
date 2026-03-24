# Dependency Injection
## Introduction

This a short guide to get started using the `DependencyContainer` of the **BizApps Core Accelerator**. It is a core concept that should be understood before writing Plugins.

## Dependency Injection

[Dependency Injection](https://en.wikipedia.org/wiki/Dependency_injection) supports the `Dependency Inversion Principle`. Instead of manually constructing  dependencies in high-level code and passing it down to the low-level components, a container is used which takes care of that. The container has to be configured once (map the interfaces to their concrete implementation, set lifetime, ...) and can then be used to construct objects and inject its dependencies.

Only a single call to the `Resolve` method of the `DependencyContainer` shall be used to construct one object and its whole dependency graph.

In the **BizApps Core Accelerator** the [**Unity**](https://github.com/unitycontainer/unity) library from [**Patterns & Practices**](https://msdn.microsoft.com/de-de/library/ff921345.aspx) is used as the dependency injection container.

## Dependency Inversion Principle

Dependency inversion means, that an object (E.g. `SomeWebService`), doesn't construct its dependencies, but instead gets them injected via constructor or property parameters.

Let's take a look at the fictitious class called `SomeWebService`, which depends on the interfaces `IWebClient` and `IConsoleWriter`. 
Without dependency injection the class would look like such:

```csharp
public class SomeWebService {
    private readonly IWebClient _webClient;
    private readonly IConsoleWriter _consoleWriter;

    public SomeWebService () {
        _webClient = new WebClient();
        _consoleWriter = new ConsoleWriter();
    }
}
```

This violates the [loosely coupling principle](https://en.wikipedia.org/wiki/Loose_coupling) as `SomeWebServie` needs to know the concrete implementations of the types `IWebClient` and `IConsoleWriter`.

In contrast the class would look like the following with dependency inversion applied:

```csharp
public class SomeWebService {
    private readonly IWebClient _webClient;
    private readonly IConsoleWriter _consoleWriter;

    public SomeWebService (IWebClient webClient, IConsoleWriter consoleWriter) {
        _webClient = webClient;
        _consoleWriter = consoleWriter;
    }
}
```

In this case the class `SomeWebService` has no idea which implementation `IWebClient` and `IConsoleWriter` are passed in. That means it has no knowledge of the definitions of other components. 

Injecting dependencies also provides the base to easily create unit tests because all dependencies can be easily mocked and passed in as constructor parameters.

## Usage of the DependencyContainer

In the **BizApps Core Accelerator** the dependency container is simply named `DependencyContainer` and is located under the `Avanade.BizApps.Core.Plugins.DependencyInjection` namespace. It is constructed once inside the `Plugin` base class, from which all plugins should derived from. Then with each call to the `Execute` method a child container is constructed. Some instances like the `IPluginExecutionContext` or the `ILogger`, which are different for each plugin invocation are then registered distinctively to its unique child container.

To make use of dependency injection you firstly need to define an interface for each dependency you want to inject. Then you need to register this interface to the concrete implementation of the dependency. You have the following possibilities to register the dependencies:

+ Interface name matches implementation name (SomeDependency -> ISomeDependency)
  + The type will be automatically registered with the interface
  + The `Lifespan` of this object will be `Transient`
  + That means  each time this type is requested it will return a new instance.
+ Interface name does not match implementation name (SomeDependency -> IMyInterface) or
+ Custom `Lifespan` needs to be configured or
+ Custom type construction method needs to be configured
  + Take a look at **How to integrate custom registration** (not necessary in 99,9% of the cases)

Then you simply need to add the dependency as a parameter to the constructor.

```csharp
public class MyClass {
    private IMyDependency _dependency;
    public MyClass(IMyDependency dependency) {
        _dependency = dependency;
    }
}
```

The registered implementation of `IMyDependency` will automatically be injected into the class by the container.

**That's it! Now you can use `IMyDependency` without worrying about constructing it.**

## How to integrate custom registration

In case the default conventions does not match your needs you can manually register types to the container.
Therefore navigate to the namespace 
 `Avanade.BizApps.Core.Plugins.DependencyInjection.Registration.Contracts`, which contains two interfaces:

+ The `IConventionDependencyRegistrationService`,
  which contains a single method called `RegisterTypesFromCollection`.
  + This interface is used when you want to iterate through all types and use custom conventions to register them with their interfaces. See **Conventions** for more information.
  + >E.g. Register each type that contains "Logger" with all interfaces instead of only the default interface "ILogger"

+ The `ICustomDepdendencyRegistrationService`, which contains a single method called `RegisterTypes`.
  + This interface can be used to register individual interfaces to their implementation. See **Individual Types** for more information
  + > E.g. Register the class `FeatureToggleService` to the interface `IFeatureToggle`
  + > E.g. Register the class `OperationFactory` to the interface `IOperationFactory` with the lifetime `Singleton`.

### Conventions

To register a convention, create a derived class from `IConventionDependencyRegistrationService` and implement the `RegisterTypesFromCollection` method. From there you can call the `container.RegisterTypes` method to iterate the types and register them.

The following example registers each type, that contains the word "logger" with all it's interfaces.
E.g. `MyCustomLogger` will be registered to `ILogger` and `IMyCustomLogger` instead of only `IMyCustomLogger`:

```csharp
public class RegisterDefaultConventions : IConventionDependencyRegistrationService
{
    public void RegisterTypesFromCollection(IUnityContainer container, IEnumerable<Type> types)
    {
        container.RegisterTypes(
            types.Where(t => t.Name.ToLower().Contains("logger")),
            WithMappings.FromAllInterfaces,
            WithName.Default,
            WithLifetime.Transient,
            null, true
        );
    }
}
```

For more information please take a look at the documentation of **Unity**.

### Individual Types

To register an individual type, create a derived class from `ICustomDepdendencyRegistrationService` and implement the `RegisterTypes` method. From within this method you can call `container.RegisterType` to map an interface to a concrete type.

E.g. The following sample code will register the interface `IFeatureToggle` to the concrete type `FeatureToggleService` as singleton per container:

```csharp
container.RegisterType<IFeatureToggle, FeatureToggleService>(new HierarchicalLifetimeManager());
```

The most important available LifetimeManagers are

| Name                               | Description                                                                                                         |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| ContainerControlledLifetimeManager | The type will be registered as singleton. Meaning only one instance of this type is shared by the whole application |
| HierarchicalLifetimeManager        | Same as `ContainerControlledLifetimeManager`, but each child container receives it's own instance                   |
| TransientLifetimeManager           | Each request to the type will return a new instance                                                                 |

For more detail about the `RegisterType` function, please take a look at the documentation of **Unity**.
