# Licensing

If you decide to use the BizApps Core Accelerator in your engagement you have to take the topics of this page into consideration.

The following topics are more important when any contract comes to an end, but they have to be part of the topics which are clarified first. Not just to protect our IP but also by providing a contractual safety net in terms of "what will be handed over to the customer".

The BizApps Core Accelerator is comprised of a SDK and tools.

## What means SDK and tools

The SDK is used to write the actual business logic. Thus it can't be "removed" from the code. The business logical and SDK code are, to a certain extend, interwoven. In contrast every tool which comes with the BizApps Core Accelerator is opt-in and can be excluded. For instance removing the Automated Plugin Registration Tool will result in the fact that plugins have to be registered the "normal" way -> manually using the Plugin Registration Tool provided by MSFT.

## What means intervowen
Depending on the layer (Fronted vs. Backend) or if you utilize any tool inside of the BizApps Core Accelerator intervowen means different things:

### SDK

The BizApps Core Accelerator SDK is used to implement the business logic. The business logic is thus intervowen with the SDK itself. Imagine you're using our frontend caching mechanic. You've built the business logic around it so the caching mechanic of the SDK is a integral part of the business logic and can't be removed.

#### Frontend
All BizApps Core Accelerator SDK related frontend librarires & code are distributed as one *npm* package. A npm package is just a wrapper around the code. This means that, if you use the BizApps Core Accelerator, the client will receive that complete code as is. We're just creating the wrapper around our code so it can be used easily without micro management of the different provided libraries.

#### Backend
The Backend SDK is distributed via a NuGet package. This packages contain pre-compiled code in form of assemblies. By that is does not contain the code so the client could not fix/maintain/build the SDK/tools by themselves. They're able to continue using the SDK to fix/extend/maintain their business logic but they're not able to fix/extend/maintain the SDK itself.

### Tools & CI/CD pipeline
Tools are for instance the automated plugin registration tool, the web resource loader or the CI/CD pipeline. Those tools are being distributed via nuget packages aswell and contain just the executables and assemblies need to do the job. These tools are project accelerators which are used by Avanade/Accenture to deliver the solution and are not part of the hand over to the customer. You should provide a clear statement that any tools used during the engagement are not part of the package which will be handed-over to the client. 

## Contracting

Before actual using the BizApps Core Accelerator in an engagement the client needs to be aware of our [License](https://dev.azure.com/innersource/DSS-Framework/_git/Template?path=%2FLICENSE.md&_a=preview). This license has been provided by our legal team and have to be part of the contract. If it's not part of it or the client didn't sign the license you can't use the Framework for multiple reasons (warranty implications, liability, protection of IP,..)

As described above there are multiple components inside of "The Framework". Depending on the component it might be removable at the end of the engagement or not.

### SDK
Because the business logic which was implemented based on the client's requirements is intervowen with the SDK we have to provide the SDK in some sort that the client can continue fixing/maintaining/extending their solution. Now the question is if it's sufficient to provide the SDK as pre-compiled assemblies or if we have to provide the complete code to the SDK itself as well. 

The current concept around this is that it would be sufficient for the purpose of fixing/maintenance/extension if we provide the SDK as pre-compiled assemblies. As of now we're missing the legal information if this would be feasable or not.

### Tools
As described above every tool is just an accelerator which we use to deliver the solution. As soon as the contract ends the tools have to be removed from the whole solution because it is NOT mandatory to continue fixing/maintaining/extending the solution. Picking up the example of the Automated Plugin Registration Tool: It automates the deployment of plugins onto any target environment. If it's not present the client could build their plugin assembly by themselves and deploy it via the MSFT Plugin Registration Tool. This applies to every tool which we provide in the BizApps Core Accelerator.

### CI/CD pipeline
Same as with the tools. As long as we're having a contract with the client we're using the pipeline to deliver the solution. If the contract ends we'll remove the whole pipeline completely.
