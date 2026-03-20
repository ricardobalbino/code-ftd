# Dataverse Development Standards

## Dataverse Configuration (Customizing)

For all configuration tasks refer to the guidelines given in the [Customizing Guidelines](https://adf.avanade.com/display/FODG/%28CRM%29+Customization) which includes policies how to setup the Publisher, solutions etc.

## Web Resources

For the general considerations regard the configuration of Web Resources
(also for images etc.), refer to the [Customizing Guidelines](https://adf.avanade.com/display/FODG/%28CRM%29+Customization). The handling of Web Resources for frontend code is described below.

In order to handle all non-code Web Resources (e.g. images) properly, add the same to the project folder *\\src\\Dataverse\\Web\\src\\web_resources* and then follow the same process for the frontend code.

## Frontend Code

### Typescript

The BizApps Core Accelerator uses transpiling for the code to work in most common browsers but the Typescript syntax must be followed.

#### Common Methods

Before creating a new method, check if there is already an existing (or similar) method defined in any of the existing models and controllers.

#### Avoidance of Single Line Blocks

Single line statements like

`return (x == y);`

are ok and encouraged but single line blocks like

```Typescript
if (x)
   return y;
```

should be avoided for better readability and such single line blocks must
be embraced with brackets like

```Typescript
if (x) {
   return y;
}
```

#### New Line After Block

When starting a new block, a line break must be added.

**Wrong**

```Typescript
if (..) {

...

} else {

...

}
```

**Correct**

```Typescript
if (..) {

...

}

else {

...

}
```

#### Value Comparison (Javascript Only)

As Javascript does not perform strict type validation by default, for any comparison the correct syntax should be applied using === or !==.

- Check if a variable/object is undefined using
  `if (typeof(myVar) !== "undefined")`
  or
  `if (!!myVar)`

- Compare two variables with
  `(myVar1 === myVar2)`
  or
  `(myVar1 !== myVar2)`
  This reduces the probability of errors and increases performance.

#### Single const/let Pattern

Usage of global variables (i.e. not using any declarative statement in a function) must be minimized.

All local variables within a function must be declared at the beginning of the function in one single statement and every variable should be initialized properly. This allows for a better readability and understanding of the code.

```Typescript
let myInt = 0,
    myBool = false,
    myString = '',
    myObj = {},
    myArr = [];
```

#### Switch Pattern

For better readability and consistency, the following pattern should be used to define switches:

```Typescript
switch (a) {
   case "1":
      alert("1");
      break;

   case "2":
      alert("2");
      break;

   default:
      alert("default");
      break;
}
```

#### Avoidance of Object and Array Constructors

New objects or array should not be initialized using the constructors *Object()* or *Array()*, because this can be misleading and it is completely unnecessary.

For objects rather use:

```Typescript
const myObject = {
  prop1: true;
  prop2: 2
};
```

For arrays use:

`const myArray = [ 1, 2, 3, 4 ];`

#### Usage of Number() for Number Conversion

Instead of using *parseInt()* one should use *Number()* to convert an object to a number.

#### Cache Variables

*Bad example:*

```Typescript
function BuildUi()
{
      var baseElement = document.getElementById('target');
      baseElement.innerHTML = ''; // Clear out the previous
      baseElement.innerHTML += BuildTitle();
      baseElement.innerHTML += BuildBody();
      baseElement.innerHTML += BuildFooter();
}
```

*Good example:*

```Typescript
function BuildUi()
{
      const elementText = BuildTitle() + BuildBody() + BuildFooter()
      document.getElementById('target').innerHTML = elementText;
}
```

#### Cache Intermediate Results

*Bad example:*

```Typescript
function CalculateSum()
{
      const lSide = document.body.all.lSide.value;
      const rSide = document.body.all.rSide.value;
      document.body.all.result.value = lSide + rSide;
}
```

*Good example:*

```Typescript
function CalculateSum()
{
      const elemCollection = document.body.all; // Cache this
      const lSide = elemCollection.lSide.value;
      const rSide = elemCollection.rSide.value;
      elemCollection.result.value = lSide + rSide;
}
```

### Code Formatting

To format React or frontend code in general, use the code formatter tool *prettier* from inside the top folder of the PCF or other component:

`npx prettier --write`

### HTML

Visual Studio (Code) with the building wizards and check features must be used to ensure high quality code.

When using Javascript inside the HTML dialogs, the same guidelines apply as defined above.

**Note:** The PowerApps Component Framework might replace the typical HTML Web Resources at some point in time (as of October 2020 it was still not possibility to create standalone components without a relation to Entity forms or views): https://docs.microsoft.com/en-us/powerapps/developer/component-framework/overview

## Backend Code

### C# Standards

#### Naming Conventions

Identifier               | Case   |Example
  ------------------------ | -------- |------------
  Enum values              | Pascal   | FatalError
  Read-only Static field   | Pascal   | RedValue
  Method                   | Pascal   | ToString
  Parameter                | Camel    | typeName
  Property                 | Pascal   | BackColor

#### Abbreviations

-   Do not use abbreviations or contractions as parts of identifier names. For example, use GetWindow instead of GetWin.

-   When using acronyms, use Pascal case or camel case for acronyms more than two characters long. For example, use HtmlButton or htmlButton.
    However, you should capitalize acronyms that consist of only two characters, such as System.IO instead of System.Io.

-   Note that when using a name which has the suffix 'Id' as in CompanyId, the 'd' must be lower case.

#### Class Naming

-   Use a noun or noun phrase to name a class.

-   Use Pascal case.

-   Use abbreviations sparingly.

-   Do not use a type prefix, such as C for class, on a class name. For
    example, use the class name FileStream rather than CFileStream.

-   Do not use the underscore character (\_).

#### Parameter Naming Guidelines

-   Use camel case for parameter names.

-   Use descriptive parameter names.

-   Do not use reserved parameters.

#### Method Naming Guidelines

-   Use verbs or verb phrases to name methods.

-   Use Pascal case.

#### Property Naming Guidelines

-   Use a noun or noun phrase to name properties.

-   Use Pascal case.

-   Consider creating a property with the same name as its underlying type. For example, if you declare a property named Color, the type
    of the property should likewise be Color.

#### Enumeration Naming Guidelines

-   Use Pascal case for Enum types and value names.

-   Use abbreviations sparingly.

-   Do not use an Enum suffix on Enum type names.

-   Use a singular name for most Enum types, but use a plural name for Enum types that are bit fields.

-   Always add the FlagsAttribute to a bit field Enum type.

#### Static Field Naming Guidelines

-   Use nouns, noun phrases, or abbreviations of nouns to name static fields.

-   Use Pascal case.

#### Code Commenting

As a general guiding principle, the code must be properly structured and the logic must be decomposed into well readable methods so that comments
would not necessary to describe the implemented logic. Comments should not just repeat what is done in the code but provide some meaningful
information about context, background and usage of the described classor function or piece of code (see also
<https://hackaday.com/2019/03/05/good-code-documents-itself-and-other-hilarious-jokes-you-shouldnt-tell-yourself/>).

Remove outcommented code.

#### General Exception Handling Strategy

-   Make use of structured exception handling (Try/Catch/Finally Block) Take note: Try/Catch block defines scope. A variable defined inside
    a Try block, will not be visible in the Catch block.

-   Handle exceptions on the appropriate level, usually the UI.

-   Do not catch exceptions only to re-throw them again, only catch and re-throw if meaningful information can be added to the error message

-   Derive custom exceptions from ApplicationException.

-   No unhandled exceptions, add a global exception handler

#### Empty Strings

Empty strings are to be declared using `string.Empty`.

#### String Comparison

For performance reasons, string values have to be compared using `Equals()`, not using `CompareTo()` or `==´.

#### String Concatenation

For performance reasons, use the *StringBuilder* class to concatenate string, because with a simple concatenation of strings via "+" multiple string objects are instantiated which is costly.

Alternatively, use the more recent C\# \$"..." syntax to build strings.

### Server-Side Code (Plug-Ins and Workflow Activities)

The BizApps Core Accelerator is utilized for backend code and provides a lot of intrinsic coding support to avoid common and Dynamics specific coding pitfalls. Nevertheless, the following topics should be always checked.

-   Only retrieve the data required for the specific operation

    -   Limit the columns retrieved (in LINQ queries this is done via the `Select` statement and you must always ensure that only the needed fields are retrieved

    -   Apply appropriate filters

    -   Use Input Parameters and available properties like BusinessUnitId / OrganizationId etc. of the Plugin Context if possible

-   Limit operations that cascade to related entities

    -   Some operations such as deletion or reassigning of a record may cause related records to do the same, this is a heavy operation and may not be necessary

    -   Do not rely on cascading if it's part of a technical rather than a business requirement to avoid unnecessary cascading operations throughout the system

    -   Use pre-validation or pre-operation where possible to minimize database updates

-   Prevent all-purpose plug-ins and workflow activities

    -   Scale and scope plugins and workflow activities to perform a single task based on a clear concise set of input and create predictable output

-   Use the tracing service provided by the platform to log execution start, end, operations, success and failure

### External Client Code

- Use an application user (Client ID and Certificate/Secret with App Registration) to authenticate with Dynamics/Dataverse

- Apply caching

    - It is not always required to establish sophisticated caching. Good results can already be achieved by using static cache objects to store retrieval results for a while.

    - For high performance requirements it might be better to use cache services like Redis

-   Only retrieve the data required for the specific operation

    -   Limit the columns retrieved

    -   Apply appropriate filters

-   Limit operations that cascade to related entities

    -   Some operations such as deletion or reassigning of a record may cause related records to do the same, this is a heavy operation and may not be necessary

    -   Do not rely on cascading if it's part of a technical rather than a business requirement to avoid unnecessary cascading operations throughout the system

-   Follow best practices and use logging/error handling like Microsoft
    Logging Extensions.

## Plug-Ins

Plug-ins are written in C\#, and therefore the standard guidelines from above apply.

#### Update of Field Values

Many of the table columns/entity fields are audited and every update is logged in the Audit History even if the value has not changed. This is because the Dataverse SDK does not have a built-in functionality to detect if there are changes to an existing value and prevent audit logging if no changes have occurred.

Therefore, when operating in the post operation stage of a plug-in, this must be handled in the plug-in code, i.e. especially for all fields in the attribute collection which are audited, it must be checked if the values have really changed before the environment service is called for an update.

Instead of updating the existing entity target in the post operation stage, just construct a new entity object containing only the entity record ID and the field values to be updates before calling `Update()`.

!!! info "Summary"
    Make sure that you only add column values as attributes which have changed before you execute the update of a record and  when working in post operation, make all updates on a new entity object and not the same entity object.

### QueryExpressions

The BizApps Core Accelerator uses LINQ queries by default but it is also possible and sometimes required to use QueryExpressions.

#### The State Code

If not stated explicitly, the state code (active or inactive) must be considered always when retrieving multiple entity records, which means that only active records should be retrieved. This means that the standard behavior and implementation characteristics of Dynamics is
followed so that for example a user can deactivate certain records instead of deleting the same and these records are not considered even in the custom functions.

#### Usage of "NoLock" property for QueryExpression objects

Set the `NoLock` property to true at all times for all *QueryExpression* objects.

#### Prohibition of "new ColumnSet(true)"

In most projects, there is no need to ever use the pattern "ColumnSet = new ColumnSet(true),". This leads to a response containing every field of the current entity (also system fields like modifiedon, createon, owner, etc.) which may not be required for the current scenario. If you
use this pattern (e.g. for cloning plugins etc.), add a comment that you used this with intent.

### HTTP Requests via HttpWebRequest Class / HttpClient

If you are making a call towards Azure, add an HTTP-Header "x-ms-client-tracking-id" with a randomly created id. This allows tracking of the run in the Azure Logic Apps or other components.

Set the `KeepAlive` attribute to false to eliminate timeout issues and dispose the `HttpWebRequest` object after each call and make the controller logic non-static.

For `HttpClient`, the `KeepAlive` property can be set via

`http.DefaultRequestHeaders.Add("Connection", "close");`

-  https://stackoverflow.com/questions/36357943/c-sharp-how-to-set-httpclient-keep-alive-to-false

-  https://docs.microsoft.com/en-us/dynamics365/customer-engagement/guidance/server/set-keepalive-false-interacting-external-hosts-plugin
