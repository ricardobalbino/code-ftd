# Handling the Target

This page provides the context on how we handle every aspect of these topics.

Every plugin relates in some manner around the `target` provided by the `IPluginExecutionContext.Inputparameters`.
Additional you can provide a pre entity image which you want to take into consideration when writing your business logic.

## How Dataverse deals with the "target"

You can find out more on how Dataverse handles the `target` [here](https://docs.microsoft.com/en-us/powerapps/developer/data-platform/understand-the-data-context).

Most of the time the `target` is an instance of type `Entity` which contains the list of attributes which the entity is comprised of. Additional the `target` which is provided in the `InputParameters` will only contain the attributes which have changed. So for example if the attributes `FirstName` and `LastName` were changed on the account form the `target` will only contain those two attributes and their respective values.

There are other cases where the `target` is not an `Entity`. On a `Delete` message the `target` is of type `EntityReference`.

Code which has to take this behaviour into consideration has a lot of conditions and asserts to validate if it has to evaluate the `target` this way or the other way. Thus the cyclomatic complexity grows exponentially by the number of occurances the code is being used in. In addition other -ilities like the maintainability or flexibility are negatively influenced by that as well.

## How the BizApps Core Accelerator deals with the "target"

### Without a Pre-Image
If you've defined on your plugin registration attribute above your plugin class to not contain a pre-image the `target` won't have a different behaviour. As described in the section before, the `target` will contain all attributes which were changed and the rest of the Entity's attributes are `null`. Again same behaviour as standard Dataverse.

### With a Pre-Image
If you registered the step with a pre-image using the `IncludePreImage = true` the record passed into the `Execute` method (second parameter) will always be a combination of the standard `target` (only containing the changed attributes) and the pre-image (contains all attributes of the record before the core platform operation). The values inside of the `target` are more current and thus are "overruling" the values of the pre-image. For example:

InputParameters -> Target contains 2 attributes:
- FirstName
- LastName

InputParameters -> Pre-Image contains 4 attributes :
- FirstName
- LastName
- Title
- Address1-Street

Plugin -> Target contains 4 attributes:
  - FirstName **(InputParameters -> Target)**
  - LastName **(InputParameters -> Target)**
  - Title **(Pre-Image)**
  - Address1-Street **(Pre-Image)**

With this behaviour you can rely on the fact that the `target` reflects the current state of the record. You don't have to micro manage back and forth between the target and the pre-image to get a holistic view of the record itself.

See how we're dealing with detecting changes of the current record [here](Detecting-changes.md).

### What's the deal with a Post-Image

The BizApps Core Accelerator is currently not utilizing Post-Images. Since the record passed into the `Execute` method is a combination of the pre image and the target this parameter is already the post-image.
