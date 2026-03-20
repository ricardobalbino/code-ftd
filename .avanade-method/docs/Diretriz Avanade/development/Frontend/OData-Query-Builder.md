# OData Query Builder

This guide contains all the relevant information to get started with the **OData Query Builder**.
It provides a fluent API to make it easy building queries even without any OData / Dataverse experience.
The fluent API orients itself on the .NET / C# LINQ Syntax.

## Basic Query Example

Using the `ODataQueryBuilder` is quite simple. 
Start by import the class from the `@avanade/dss-sdk` npm package like the following:

```ts
import { ODataQueryBuilder } from "@avanade/dss-sdk";
```
In addition you need to import the entity you want to query from your project files.
They are usually located at `Project/Web/generated/entities`.

In this example I'm going to use the `Contact` entity:

```ts
import { Contact } from "../../../generated/entities/contact";
```

Now we are finished importing all the required classes and can continue by writing our first query. The following example illustrates the basic usage:

```ts
const builder = ODataQueryBuilder
    .for(Contact)      // Set the entity you want to query
    .select((c: Contact) => [     // Set the attributes you want to select
        c.FirstName, 
        c.LastName
    ])
    .selectSingle(someContactId); // Query a single entity by it's ID

const myContact = await builder.execute(myGlobalContext); // Executes the query

console.log(`FirstName: ${myContact.FirstName}, LastName: ${myContact.LastName}`);
```

> Note: This example utilizes modern JavaScript Features (ES2015+), like [Template Literals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals), [Arrow Functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions) and [async](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function) / [await](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await). Be sure to make yourself comfortable with these new features, as they are heavily used in the BizApps Core Accelerator.


## Set the entity

The `for([TEntity])` function is required to be called on each query, as it tells the `ODataQueryBuilder` which entity to query.

```ts
QueryBuilder.for(Contact)...
```

### Request specific properties

The `select([function(TEntity): Array<AttributeBase>])` function can be used to query a specific column set of the entity.

```ts
QueryBuilder.for(Contact).select((c: Contact) [
    c.FirstName, 
    c.LastName
])...
```

This also support querying related entities:

```ts
QueryBuilder.for(Contact).select((c: Contact) => [
    c.FirstName, 
    c.LastName,
    c.Relations.contact_customer_accounts.Address1_Line1,
    c.Relations.contact_customer_accounts.EMailAddress1
])...
```
### Filter result set 

You can use the `where([function(TEntity): Array<AttributeFilterBase>])` function to set a criteria for which entities are returned.

This method can only be used when retrieving multiple entities.

```ts
QueryBuilder.for(Contact).select(...).where((c: Contact) => [
    c.FirstName.asFilter().equal("Simon")
])...
```

This also support multiple conditions, separated by a `QueryOperator`.

```ts
QueryBuilder.for(Contact).select(...).where((c: Contact) => [
    c.FirstName.asFilter().equal("Simon"),
    QueryOperators.And,
    c.LastName.asFilter().equal("Bauer"),
])...
```

Moreover the conditions can be grouped using a `FilterGroup`.

```ts
QueryBuilder.for(Contact).select(...).where((c: Contact) => [
    c.FirstName.asFilter().equal("Simon"),
    QueryOperators.Or,
    new FilterGroup([
        c.FirstName.asFilter().equal("Markus"),
        QueryOperators.And,
        c.LastName.asFilter().equal("Kellenbenz"),
    ])  
])...
```

### Order the result set

Use the `orderby([function(TEntity): Array<AttributeOrder>])` function to specify one or more attributes, which will be used to sort/order your query.

```ts
QueryBuilder.for(Contact).select(...).orderby((c: Contact) => [
    c.FirstName.asOrder().descending()
])...
```

### Limit results

Use the `take([Integer])` method to set the maximum amount of entities returned by the query.
This method is only available when querying multiple entities (not available when using `selectSingle(...)`).

```ts
QueryBuilder.for(Contact).select(...).take(1)...
```

### Select a single Entity by its ID

You can use the `selectSingle([String])` method to only select a single entity. The parameter of this function has to be a valid guid in the format `XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX`.

```ts
QueryBuilder.for(Contact).select(...).selectSingle("9288A624-5369-442E-BB64-471F268A69C3");
```

### Execute the Query

Use the  `execute([GlobalContext])` method to build and execute your query. The result will be mapped to the entity given by the `for([Entity])` method.

```ts
const result = await QueryBuilder.for(Contact).select(...).execute(myGlobalContext);
```

## Examples


### Simple Select (Single Entity) 

```ts
const queryBuilder = ODataQueryBuilder
    .for(Contact)
    .select((c: Contact) => [
        c.FirstName,
        c.LastName
    ]).selectSingle("9288A624-5369-442E-BB64-471F268A69C3");

const myContact = await queryBuilder.execute(myGlobalContext);
console.log(`FirstName: ${myContact.FirstName}, LastName: ${myContact.LastName}`);
```

### Select Relations (Single Entity)

```ts
const queryBuilder = ODataQueryBuilder
    .for(Contact)
    .select((c: Contact) => [
        c.FirstName,
        c.LastName,
        c.Relations.contact_customer_accounts.Address1_Line1
    ]).selectSingle("9288A624-5369-442E-BB64-471F268A69C3");

const myContact = await queryBuilder.execute(myGlobalContext);
console.log(`Account Address Line 1: ${myContact.contact_customer_accounts.Address1_Line1}`);
```

### Filters (Select Multiple)

```ts
const queryBuilder = ODataQueryBuilder
    .for(Contact)
    .select((c: Contact) => [
        c.FirstName,
        c.LastName
    ]).where((c: Contact) => [
        c.FirstName.asFilter().equal("Simon")
    ]);

const myContacts = await queryBuilder.execute(myGlobalContext);
for(const myContact of myContacts) 
    console.log(`FirstName: ${myContact.FirstName}, LastName: ${myContact.LastName}`);
```

### Order By (ascending)

```ts
const queryBuilder = ODataQueryBuilder
    .for(Contact)
    .select((c: Contact) => [
        c.FirstName,
        c.LastName
    ]).orderby((c: Contact) => [
        c.FirstName.asOrder().ascending()
    ]);

const myContacts = await queryBuilder.execute(myGlobalContext);
for(const myContact of myContacts) 
    console.log(`FirstName: ${myContact.FirstName}, LastName: ${myContact.LastName}`);
```

### Order By (descending)

```ts
const queryBuilder = ODataQueryBuilder
    .for(Contact)
    .select((c: Contact) => [
        c.FirstName,
        c.LastName
    ]).orderby((c: Contact) => [
        c.FirstName.asOrder().descending()
    ]);

const myContacts = await queryBuilder.execute(myGlobalContext);
for(const myContact of myContacts) 
    console.log(`FirstName: ${myContact.FirstName}, LastName: ${myContact.LastName}`);
```

### Take

```ts
const queryBuilder = ODataQueryBuilder
    .for(Contact)
    .select((c: Contact) => [
        c.FirstName,
        c.LastName
    ]).take(5); // Will only return 5 results

const myContacts = await queryBuilder.execute(myGlobalContext);
for(const myContact of myContacts) 
    console.log(`FirstName: ${myContact.FirstName}, LastName: ${myContact.LastName}`);
```

## Caching of odata response

You can cache odata responses by providing an additional parameter for the `.execute` function.

```ts
    return command.execute("SampleCommand", parameters, Expires.inMinutes(5));
```

The `Expires` helper class provides different methods to add timespans.