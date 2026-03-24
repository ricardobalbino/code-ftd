# Early- vs late-bound

## Introduction

Until this day there are a lot of rumors and prejudice around/against early-bound. The "story" sounds something like this: "Early bound is not as optimal as late bound in terms of performance."

The source of this bias comes from some benchmarks back in 2013 for CRM 2011. Since then a lot has happened and both Dynamics and C# as a language have improved and evolved.

## The test
We've created the example benchmark code which you can see at the bottom of this page. Each benchmark (early / late) was executed 100 times with a warm up phase of 3 executions.

## The result

Based on the below benchmark code and the above benchmark setup these are the results:

|         Method |     Mean |    Error |   StdDev |      Min |      Max |   Median |
|--------------- |---------:|---------:|---------:|---------:|---------:|---------:|
| TestEarlyBound | 433.4 ms | 22.52 ms | 64.98 ms | 320.8 ms | 609.4 ms | 416.4 ms |
|  TestLateBound | 436.5 ms | 23.43 ms | 68.34 ms | 333.4 ms | 621.5 ms | 418.8 ms |#

As you can see, the early bound benchmark was "faster" in for all methods but "faster" meaning under 1%.

## Conclusion
The difference between early- vs. late-bound is neglectable and there's no reason not using early-bounds entities especially because early-bound brings a lot more to the table in terms of maintainabilty and quality.

## Benchmark code

```csharp
using System;
using Avanade.BizApps.Core.Common.Entities;
using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Configs;
using BenchmarkDotNet.Running;
using Microsoft.Crm.Sdk.Messages;
using Microsoft.Xrm.Sdk;
using Microsoft.Xrm.Tooling.Connector;

namespace DynamicsBenchmarkEarlyVsLate
{
    public class Program
    {
        public static void Main(string[] args)
        {
            BenchmarkSwitcher.FromAssembly(typeof(Program).Assembly).Run(args, new DebugInProcessConfig());
        }
    }

    [MinColumn, MaxColumn, MeanColumn, MedianColumn]
    public class DynamicsTester
    {
        private static readonly string ConnectionString = "XXX";

        private static CrmServiceClient _serviceClient;

        public DynamicsTester()
        {
            _serviceClient = new CrmServiceClient(ConnectionString);
            if (_serviceClient.IsReady)
            {
                WhoAmIResponse response =
                    (WhoAmIResponse)_serviceClient.Execute(new WhoAmIRequest());

                Console.WriteLine("User ID is {0}.", response.UserId);
            }
            else
            {
                throw new InvalidOperationException("Can't connect to Dataverse");
            }
        }

        [Benchmark]
        public void TestEarlyBound()
        {
            var contact = new Contact()
            {
                FirstName = "MRA",
                LastName = "Test",
                Address1_City = "Address1_City",
                Address1_Line1 = "Address1_Line1",
                Address1_Line2 = "Address1_Line2",
                Address1_Line3 = "Address1_Line3",
                Telephone1 = "Telephone1"
            };

            _serviceClient.Create(contact);
        }

        [Benchmark]
        public void TestLateBound()
        {
            var entity = new Entity
            {
                LogicalName = "contact",
                ["firstname"] = "MRA",
                ["lastname"] = "Test",
                ["address1_city"] = "Address1_City",
                ["address1_line1"] = "Address1_Line1",
                ["address1_line2"] = "Address1_Line2",
                ["address1_line3"] = "Address1_Line3",
                ["telephone1"] = "Telephone1"
            };

            _serviceClient.Create(entity);
        }
    }
}
```
