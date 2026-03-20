# Azure Service Bus Service

The service provides a streamlined and performance-optimized way to send messages over the Service Bus (or as a Webhook) from Dataverse plug-ins.

It uses the Dynamic Expression service to effectively render the object to be sent over, i.e. there is only one retrieval required to assemble all data including referenced fields (which are typically not contained in the execution context) which also avoids callbacks into Dataverse to retrieve additional data.

As the standard Service Bus registration in Dataverse is leveraged, the actual message is contained in the `InputParameter` "CanonicalMessage" as a serialized string. The rest of the message is the general execution context. This means processing components just need to take the "CanonicalMessage" string and deserialize the string into the actual message object.

# Typical Use Cases

* Any asynchronous message integration

# Methods

`void SendMessage<T>(string serviceEndpointName, EntityReference entityReference, IMessageProcessingService<T> messageProcessingService, List<string> fullNamesOfUsersToSkipExecution)`

`void SendMessage<T>(Guid serviceEndpointId, EntityReference entityReference, IMessageProcessingService<T> messageProcessingService, List<string> fullNamesOfUsersToSkipExecution)`

Both methods send a message to the registered Azure Service Bus using the InputParameter "CanonicalMessage", either by directly getting the Service Bus Endpoint Guid or the named (which is then cached to achieve high performance without having to maintain the Guids in the configuration).

`IMessageProcessingService` can be implemented and optionally  provided as `messageProcessingService` to add some post-processing to the message after rendering, for example to add child objects.

`fullNamesOfUsersToSkipExecution` can be optionally provided to skip the sending of a message for example when a message is sent on record creation but should not happen when the record is created during an integration by an Integration User.

# Examples

This example shows a simplified message to send the case title.

## Message Class

```csharp
public class SendSimplifiedCase
{
   [ResolveFromDynamicExpression(Expression = "{!incident:title;}")]
   public string Title { get; set; }
}
```

## Case Plug-In

```csharp
...
_azureServiceBusService.Send<SendSimplifiedCase>("case-queue", target.ToEntityReference(), null, null);

...
```

>**Note:** Service bus endpoints can be registered easily via the `<RegisterServiceBusEndpoints>` Command in `PostSteps.xml`.

## Azure Function

```csharp
public class DynamicsServiceBusExecutionContext
{
  public List<KeyValue> InputParameters { get; set; }
}

[FunctionName("MessageListenerDynamicsCreateNotification")]
public async Task RunDynamicsCreateNotification([ServiceBusTrigger("case-queue", Connection = "ServiceBusConnectionString")] DynamicsServiceBusExecutionContext message,
                            [DurableClient] IDurableOrchestrationClient client)
{
  SendSimplifiedCase sendSimplifiedCaseMessage = JsonConvert.DeserializeObject<SendSimplifiedCase>(message.InputParameters.First(i => i.Key == "CanonicalMessage").Value.ToString());
  ...
}
```
