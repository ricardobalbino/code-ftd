# Early Bound Entities (Frontend)

The **BizApps Core Accelerator** provides an out-of-the-box way to generate Early Bound Entities for the Frontend / JavaScript. The tool will generate an [EcmaScript2015 Class](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes) for each entity you want to be generated. The classes contains all attributes and relations of the given entity. This enables IntelliSense ([Code Completion](https://en.wikipedia.org/wiki/Intelligent_code_completion)) and prevents typos in the code. In addition changes to the entity will be visible in the code and therefore breaking changes can be detected instantly.

By default generated frontend entities are located at `Project/Web/src/generated/odata`. This folder contains a file for each generated entity. E.g. `contact.js` and `account.js`. 

## How to generate new early bound entities

In the directory `Project/Web/src/generated` you can find a `config.json` file and a powershell script called `GenerateFrontend.ps1`. Simply execute the powershell script to update the existing generated entities. In case you want to add a new entity you have to modify the `config.json` file, which looks something like this:

```json
{
  "generateOptionSets": true,
  "entities": [
    {
      "logicalName": "account",
      "generateOdata": true,
      "forms": [...]
    }
  ]
}
```

To generated a new entity you have to add an entry to the `entities` array, which holds the logical name of the entity. Make sure that the `genearteOdata` property is set to `true`. In the following example I've added the `contact` entity:

```json
{
  "generateOptionSets": true,
  "entities": [
    {
      "logicalName": "account",
      "generateOdata": true,
      "forms": [...]
    },
    {
      "logicalName": "contact",
      "generateOdata": true
    }
  ]
}
```


## System Configuration

Like all tools in the **BizApps Core Accelerator** the frontend entity generation utilizes the Environment Config to determine the Dataverse instance to connect to. Simple switch the respective git branch and run the powershell script again to generate the entities for another branch.
