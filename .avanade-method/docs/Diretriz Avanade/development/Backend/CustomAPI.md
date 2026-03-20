# Custom API

The newly added `Custom API` can be used to interact with the solutions domain from outside as well as inside of plugins. The advantages of Custom APIs can be seen when calling them through LogicApps or Power Automate. The detailed definition of the custom api as well as its request & response parameters enables the easy integration of them in the mentioned services.

You can read about how to create a custom api [here](https://docs.microsoft.com/en-us/powerapps/developer/data-platform/custom-api).

## Creating a plugin for a custom api

To create a custom api aware plugin open the respective plugin project of the solution in which is should be implemented. Create a new class and replace the placeholder s `{...}` with your custom api values.

```csharp
using System;
using System.Runtime.Serialization;
using Avanade.BizApps.Core.Extensions;
using Avanade.BizApps.Core.Plugins;
using Avanade.BizApps.Core.Plugins.DependencyInjection;
using Avanade.BizApps.TemplateProject.BusinessLogic.Services;
using Microsoft.Xrm.Sdk;

namespace Avanade.BizApps.TemplateProject.Plugins.ExampleSolution
{
[PluginRegistration]
public class {CustomApiName}Plugin : CustomApiPlugin<{CustomApiName}Request, {CustomApiName}Response>, IPlugin
{
    public override {CustomApiName}Response Execute(IDependencyContainer container, {CustomApiName}Request request)
    {
        try
        {
            return new {CustomApiName}Response { Success = true };
        }
        catch (Exception e)
        {
            return new {CustomApiName}Response
            {
                Success = false,
                Error = e.FormatMessage()
            };
        }
    }
}
```

For custom api plugins we've provided a special base class called `CustomApiPlugin<TRequest, TResponse>`. The first generic parameter represents the request- and the second generic parameter the response class.

The `CustomApiPlugin<TRequest, TResponse>` base class will take care of:
- extracting the values from the `PluginExecutionContext.InputParameters` based of the properties defined in the request class
- setting the values into the `PluginExecutionContext.OutputParameters` based of the properties defined in the response class

### Request Class

You have to create a request class which the `CustomApiPlugin<TRequest, TResponse>` base class can use to extract the values from the `PluginExecutionContext`. The base class will use reflection to find all properties defined in the request class having the `[DataMember(Name = "...")]` attribute. 

An example class looks like the following:

```csharp
public class SampleCustomApiRequest
{
    [DataMember(Name = nameof(ContactId)]
    public Guid ContactId { get; set; }

    [DataMember(Name = "DifferentParameterName")]
    public string SomeOtherParameters { get; set; }
}
```

You can't use all types available in C#. You can check the available types for a custom api request parameter [here](https://docs.microsoft.com/en-us/powerapps/developer/data-platform/customapirequestparameter-table-columns). 

!!! note "Commands don't have limitations"
    `Commands` do not have any limitations for request- and response parameters - you can define even complex data structures. 

### Response Class

Similar to the request class you have to create a response class. It also needs to have the properties defined with the `[DataMember(Name = "...")]` attribute. 

The class should look like the following:

```csharp
public class SampleCustomApiResponse
{
    [DataMember(Name = nameof(Success))]
    public bool Success { get; set; }

    [DataMember(Name = nameof(Error))]
    public bool Error { get; set; }
}
```

The same constraints/principles on paramaters from the request class apply to the response class as well (does not apply to `Commands`).

#### How to return values

A difference in how custom api plugins behave in comparrison to "normal" plugins is that the `Execute` method demands a return value.
This return value is the response class. Simply construct the response class inside of the plugin and return (as seen above).

### Registration

As shown above, only the attribute `[PluginRegistration]` is needed to let the plug-in registration tool know that it has to deploy the plug-in type. The actual linking to the Custom API is done through the respective customizations on Customization Master.