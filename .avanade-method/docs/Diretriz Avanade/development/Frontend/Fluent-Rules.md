# Fluent Rules

When writing Form Scripts in JavaScript for Dataverse it can get messy pretty fast. You need to register multiple callbacks for the attributes. This can either be done directly in the customizing (big overhead) or can be done programmatically from within the onLoad form handler. Both of these methods have disadvantages. When registering any attribute handler in the customizing you quickly loose the overview of which event handler is registered for which attribute. Doing the event handler registration in code often have the same problem, as the registrations can  be splitted across classes, files and modules.

The huge problem with this is the maintenance! You simply loose the overview over which handlers are registered to which attribute, what they are doing and which side effect occur from it. It can take hours to look at the customization, analyze the code and understand how the value of an attribute is calculated. This can be very painful!

To solve this issue we developed the so called **Fluent Rules**. They provide a new approach how to write Form Scripts for Dataverse, which is easy to learn, powerful and verbose. The biggest advantage is that you can clearly see the execution paths of your form script and analyze the dependencies of specific attributes. Within seconds you can determine how a value is computed for a specific attribute!

## Concept

As the name suggests the **FluentRules** are based on a fluent interface, which makes them easy to read and write. **Fluent** rules do not contain the business logic of your form script, but simply glue together different models to interact with the Dataverse Form. 

A **FluentRule** targets a single attribute on the form and handles a specific part of the attribute data. E.g. a so called `DisplayRule` is used to set the visibility of an attribute and a `ValueRule` can be used to set the actual value of an attribute.

FluentRules are normally dependent on models or attributes. Each time one of the dependencies changes the value the **FluentRule** is re-evaluated. In addition you can also specify that a rule shall be executed when loading the page.

## Quick Start

When setting up a new **Avanade BizApps Core Accelerator** project there are already some examples included, that use the fluent rules. Mainly the file `src/forms/Account/Form/account.form.controller.js`. The **FluentRules** are grouped by their rule type into different functions, to keep the readability high. Let's pick out an easy **FluentRule** to begin with:
```ts
this._ruleFactory.createValueRule()
    .for(this._form.CreditOnHoldAttribute)
    .triggeredby(this._form.CreditLimitAttribute)
    .returns(() => this._form.CreditLimitAttribute.getValue() > 10000);
```

So let's analyze this rule:
- This is a `ValueRule`, meaning it will update the actual value of an attribute based on some other data.
- With the `for` method we specify the attribute, which this rule is going to target. In this case we target the `CreditLimitOnHold` attribute.
- Use the `triggeredBy` function to set dependencies of this value rule. Here it's dependent on the `CreditLimit` attribute.
- Use the `returns` function to register an action that returns the actual value that shall be set on the attribute

To sum up: We have a ValueRule, which updates the value of the `CreditLimitOnHold` attribute any time the `CreditLimit` attribute is changed. This change can either be done by a user on the form or by another **FluentRule** (It really doesn't matter). The CreditOnHold attribute is set to `Yes` when the credit limit is higher than 10,000, otherwise it is set to `No`.

## Rule Types

There are quite a few types of **FluentRules** available to modify different data of this attribute.

### Enable Rule

> Used to disable or activate an attribute

```ts
this._ruleFactory.createEnableRule()
    .for(this._form.CreditOnHoldAttribute)
    .triggeredby(this._form.CreditLimitAttribute)
    .returns(() => this._form.CreditLimitAttribute.getValue() <= 10000);

// The function passed to "return" has to return true or false
// true = Attribute is enabled;  false = Attribute is disabled
```

### Display Rule

> Used to change the visibility of an attribute

```ts
this._ruleFactory.createDisplayRule()
    .for(this._form.CreditOnHoldAttribute)
    .triggeredby(this._form.CreditLimitAttribute)
    .returns(() => this._form.CreditLimitAttribute.getValue() <= 10000);

// The function passed to "return" has to return true or false
// true = Attribute is visible;  false = Attribute is hidden
```

### Require Rule

> Used to change the required level of an attribute (required/none)

```ts
this._ruleFactory.createRequireRule()
    .for(this._form.CreditOnHoldAttribute)
    .triggeredby(this._form.CreditLimitAttribute)
    .returns(() => this._form.CreditLimitAttribute.getValue() <= 10000);

// The function passed to "return" has to return true or false
// true = Attribute is required;  false = Attribute is not required
```


### Value Rule

> Used to change the value of an attribute

```ts
this._ruleFactory.createValueRule()
    .for(this._form.CreditOnHoldAttribute)
    .triggeredby(this._form.CreditLimitAttribute)
    .returns(() => this._form.CreditLimitAttribute.getValue() <= 10000);

// The function passed to "return" has to return a value,
// that is compatible to the attribute type
```


### Notification Rule

> Used to display a notification on the form or attribute

```ts
this._ruleFactory.createNotificationRule()
    .for(this._form)
    .triggeredby(this.CreditLimitAttribute)
    .isLevel(NotificationLevel.Warning)
    .if(() => !this.CreditLimitAttribute.getValue() < 100)
    .returns(() => "The credit limit is too low!");

// for:     Pass either the whole FormObject or an attribute
// isLevel: Specify a notification level (Info, Warning, Error)
// if:      A function that determines if the notification shall be shown or not
// returns: The text of the notification
```

### Subscribe Rule

> Used to execute code based on a trigger

Subscribe rules are a bit special, as they do not target an attribute directly. Instead they update a model, by calling a specified function on it. The model can then e.g. make asynchronous odata calls and stuff like this. Once the model has gathered the data the **FluentRules** engine will trigger a change event on that model. This triggers all **FluentRules**, that are dependent on this model.

```ts
this._contactModel = new ContactModel(this.dynamicsContext);

this._ruleFactory.createSubscribeRule()
    .for(this._contactModel)
    .triggeredby(this._form.PrimaryContactIdAttribute)
    .executes(() => this._contactModel.loadData(this._form.PrimaryContactIdAttribute));

// The subscribe rule above calls the "loadData" method of the "ContactModel" each time the "PrimaryContactId" changes
// Another Rule can then depend on this "ContactModel". Like the DisplayRule shown below

this._ruleFactory.createDisplayRule()
    .for(this._form.CreditLimitAttribute)
    .triggeredby(this._contactModel)
    .returns(() => this._contactModel.HasContact);

```

# Modifiers

There are some global modifiers available, that can be used on most of the **FluentRules**.

| Name             | Description                                                                              |
|------------------|------------------------------------------------------------------------------------------|
| .onLoad()        | Executes the **FluentRule** when the page loads                                          |
| .onSave()        | Executes the **FluentRule** when the page is saved                                       |
| .forFormType(...)| Specifies the FormTypes in which the **FluentRule* is executed. e.g. "2" for "UpdateForm"|

The most important one is by far the `.onLoad()` modifier!

# Examples

An example form script can be found in your repository for the contact form (`src\entity\Contact\Forms\contact.contact.form.controller.ts`)