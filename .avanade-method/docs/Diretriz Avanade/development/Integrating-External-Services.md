# Integrating External Services

## Introduction

Besides writing Plugins and JavaScript to customize the Dataverse environment, there are often additional windows services or workers running in the Background. Moreover there might be external services, which need to connect to the CRM System. For this, the BizApps Core Accelerator provides an additional NuGet package named **Avanade.CRM.Core.IntegrationLayer**. This package provides a common entry point, which initialized the [Dependency Container](Backend/Dependency-Injection.md), the CrmServiceClient and additional things, like logging. Using this entry point, you can then use the same concept from the plugin development for developing the external services. This includes [Dependency Injection](Backend/Dependency-Injection.md), [Repositories](Backend/CRUD-operations.md), [Logging](Backend/Logging.md) abd [Early Bound Entities](Backend/Early-Bound-Entity-Classes.md).

## Getting started

This section will guide you through the process of creating your own external service.

1. First of you need a .NET Console Line Application, with at least .NET 4.6.2. This can easily be created from within Visual Studio.
   > **Current Limitation**: The assembly name and assembly namespace must start with "Avanade.", e.g. "Avanade.CRM.TestService"
2. Now add the **Avanade.CRM.Core.IntegrationLayer** NuGet package as a reference. Make sure you've added the [DSS Feed](https://dev.azure.com/innersource/DSS-Framework/_packaging?_a=feed&feed=DSS%40Release) to your NuGet sources list, otherwise you won't find the package.
3. Populate **App.config** with your connection string:
   To do this, simply edit the **App.config** and add the following node inside the `configuration` section and change `YOUR_CONNECTION_STRING` to the correct connection string
   ```xml
   <appSettings>  
     <add key="CRM" value="YOUR_CONNECTION_STRING"/>
   </appSettings>  
   ```
4. Create a file called `NLog.config` inside the root of your project with the following content:
   ```xml
	<?xml version="1.0" encoding="utf-8" ?>
	<nlog xmlns="http://www.nlog-project.org/schemas/NLog.xsd" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
	  <targets>
		<target name="logfile" xsi:type="File" fileName="file.txt" />
		<target name="logconsole" xsi:type="Console" />
	  </targets>
	  <rules>
		<logger name="*" minlevel="Debug" writeTo="logconsole" />
		<logger name="*" minlevel="Debug" writeTo="logfile" />
	  </rules>
	</nlog>
   ```
5. Go to the properties of the file `NLog.config` and set `Copy to Output Directory` to `Copy always`
6. Create your entry point, in this case I chose the name "MyTestApplication"
   ```csharp
   class MyTestApplication : IApplication {
       public void Execute() {
           // Put your code here
       }
   }
   ```
7. Create your startup configuration and call it from your **Main** method
   ```csharp
   class ApplicationStartup : DSSStartup<MyTestApplication> {}

   class Program {
       static void Main(string[] args) {
          var startup = new ApplicationStartup();
          startup.Execute();
       }
   }
   ```
8. That's it!

## Write Logic

Now that you have successfully setup the service project we can continue by adding some logic.

To do this we jump to the **MyTestApplication**, which is the entry point for the logic. This class is resolved using the [Dependency Container](Backend/Dependency-Injection.md), so you can use constructor injection to e.g. get a logger or `CrmServiceClient`:

```csharp
class MyTestApplication : IApplication
{
    private readonly ILogger _logger;
    private readonly CrmServiceClient _crmServiceClient;

    public Application(ILogger logger, CrmServiceClient crmServiceClient)
    {
        _logger = logger;
        _crmServiceClient = crmServiceClient;
    }

    public void Execute()
    {
        _crmServiceClient.Create(new Entity("contact")
        {
            ["firstname"] = "Test FirstName",
            ["lastname"] = "Test LastNAme"
        });

        _logger.TraceInformation("Created Contact");
    }
}
```

From this entry point you can continue writing your code just like in the plugins. When you need a service or repository, simply add it as an constructor argument and it will be injection automatically.
> When you want to use the EarlyBound entities and/or the Repositories, you have to add a project reference to the "Avanade.Project.Xrm" assembly from your plugin solution. This contains all the early bound entities, that you'll need!