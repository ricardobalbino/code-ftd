# Commands

`Commands` are a concept of the **BizApps Core Accelerator**, which enables easy communication between the Frontend and the Backend. It let's you write complex logic in the backend using C#, which is simply executed by the JavaScript code. The concept provides bi-directional data transfer, meaning you can pass in parameters to a given command and get a result once it's finished.

The concept itself is comparable to Dataverse [custom apis](https://docs.microsoft.com/en-us/powerapps/developer/data-platform/custom-api). It revolves around utilizing normal Entity interaction using Create/Read operations. 

`Commands` are compared to Custom APIs a more lean and code driven approach instead of creating first-class messages and manual customizations (creation of the custom api and it's request/response parameters). Additionally invoking `Commands` from the frontend is easier to implement (one liner) compared to custom apis message/url syntax.

## When to use a Command

There are several reasons why you should use a command:
+ Increase Business Logic readability compared to Javascript
+ Increase Performance
+ Reduce the amout of requests
+ Transaction & rollback on error
+ Act as an external API / Anti-Corruption Layer

## Creating a Command

Creating a command is pretty simple. Just navigate to the `Avanade.Project.Xrm.Commands` assembly inside the Project Visual Studio Solution. There you have to create three new classes:

1. Request class (Values going in)
2. Response class (Values going out)
3. Command class (Contains business logic)

### Request Class

You have to create a request class to which the parameters are deserialized. To utilize standard C# (De-)Serialization capabilities the request class has to have the `DataContractAttribute` on class level and a `DataMemberAttribute` for each property. An example class looks like the following:

```csharp
[DataContract]
public class SampleCommandRequest
{
    [DataMember]
    public Guid ContactId { get; set; }

    [DataMember]
    public string SomeOtherParameters { get; set; }
}
```

There are currently no limitations on which data types you can specify. You could even define a complex data type (e.g. custom class, List<Account>, etc.) as long as the targeted type is serializable by either providing `DataContractAttribute` on class level and a `DataMemberAttribute` on every parameter itself again or being serializable by default.

### Response Class

Similar to the Request class you have to create a type for the command result. It also needs to have the `DataContractAttribute` and a `DataMemberAttribute` for each property. The class should look like the following

```csharp
[DataContract]
public class SampleCommandResponse
{
    [DataMember]
    public bool IsValid { get; set; }
}
```

The same constraints/principles on paramaters from the request class apply to the response class as well.

### Command Class

Finally you need to create a command class which holds the logic of the command. It's simply another class deriving from `JsonCommandBase<TRequest, TResponse>`, where you replace `TRequest` with your **Request Class** and `TResponse` with your **Response Class**. 

Similar to the `Execute` method of an `Operation` the `Execute` method of a Command is the serialized representation of the data which will be passed in from the frontend. In this particular case you will get an instance of the class `SampleCommandRequest` passed as a parameter.

The whole sample command class looks like the following:

```csharp
[Command(CommandNames.SampleCommand)]
public class SampleCommand : JsonCommandBase<SampleCommandRequest, SampleCommandResponse>
{

    public SampleCommand(IJsonCommandSerializer commandSerializer, ILogger logger) : base(commandSerializer, logger)
    {
    }

    public override SampleCommandResponse Execute(SampleCommandRequest request)
    {
        // Execute logic and return response
    }
}
```

### Dependency injection applied

The **Command Class** will be constructed using the [Dependency Container](Dependency-Injection.md), so the same principles of DI apply to the `Command` itself as it applies to an `Operation`. You can add your dependencies as a constructor parameter and they will be injected automatically. 

```csharp
[Command(CommandNames.SampleCommand)]
public class SampleCommand : JsonCommandBase<SampleCommandRequest, SampleCommandResponse>
{
    private readonly ISomeDependency _someDependency;

    public SampleCommand(IJsonCommandSerializer commandSerializer, ILogger logger, ISomeDependency someDependency) : base(commandSerializer, logger)
    {
        _someDependency = someDependency;
    }
}
```

#### How to return values to the frontend

Because you defined that the `Command` class has a specific `SampleCommandResponse` you can simply return an instance of the class as the return value of the `Execute` method.

```csharp
public class SampleCommand : JsonCommandBase<SampleCommandRequest, SampleCommandResponse>
{
    public override SampleCommandResponse Execute(SampleCommandRequest request)
    {
        // Execute logic
        return new SampleCommandResponse { IsValid = myBooleanVariable };
    }
}
```

Because the response class itself was being defined as serializable (DataContract, DataMember) we can simply deserialize the instance of the class and write the value to the current records result attribute. This will be picked up by the frontend for further processing.

#### How to specify which command will be invoked when

The Command class itself has to have a `CommandAttribute`, which holds the unique command name, which will be used by the frontend to execute a specific request. The frontend will provide the magic string for this particular Command so the CommandDispatcher is able to find the right Command.

To eliminate typos in the command name, there is a static class called `CommandNames`, that holds all the unique names. It looks like the following:

```csharp
public static class CommandNames
{
    public const string SampleCommand = "SampleCommand";
}
```

That's it! You are now able to continue with the javascript implementation.

## Invoking a Command from the client

Commands can be executed from JavaScript using the `ODataCommand` class and the unique command name.

Start by import the class from the `@avanade/dss-sdk` npm package like the following:

```ts
import { ODataCommand } from "@avanade/dss-sdk";
```
With this import you are now able to create such command and invoke it with the specific command name specified while implementing the Command class in the backend. Look at the following example implementation of a SampleCommandModel which invokes the Command class.

```ts
import {ODataCommand} from "@avanade/dss-sdk";

export class SampleCommandController {
    private readonly dynamicsContext: DynamicsContext;

    constructor(dynamicsContext: DynamicsContext) {
        super();

        this.dynamicsContext = dynamicsContext;
    }

    async execute(contactId: string) {
        const command = new ODataCommand(this.dynamicsContext.globalContext);

        const parameters = {
            ContactId: contactId,
            SomeOtherParameters: "ADDITIONAL PARAMS"
        };

        const result = await command.execute("SampleCommand", parameters);
        if (!result.IsValid) {
            throw "Not valid"
        }
    }
}
```

The `command.execute(...)`method returns a [Promise which can be awaited](https://javascript.info/async-await).

## Caching of command results

You can cache results returned from Commands which contain stable data simply by providing an additional parameter for the `.execute` function.

```ts
    return command.execute(this.dynamicsContext.globalContext, Expires.inMinutes(5));
```

The `Expires` helper class provides different methods to add timespans.