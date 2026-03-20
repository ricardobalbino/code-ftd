# Early Bound Forms

In addition to generating [**Early Bound Entities**](Early-Bound-Entity-Classes.md) the JS Templating tool can also generate early bound form classes. These contain form attributes, tabs and sections of the specific form. This enables [IntelliSense](https://docs.microsoft.com/en-us/visualstudio/ide/using-intellisense?view=vs-2017) and prevents typos in the code. Moreover breaking changes to an entity form will be visible in the code and breaking changes can be detected faster.

## Usage

In the directory `Project/Web/src/generated` you can find a `config.json` file and a powershell script called `GenerateFrontend.ps1`. Simply execute the powershell script to update the existing generated forms. In case you want to add a new entity you have to modify the `config.json` file, which looks something like this:

```json
{
  "generateOptionSets": true,
  "entities": [
    {
      "logicalName": "account",
      "generateOdata": true,
      "forms": [
        {
          "formType": "Main",
          "formName": "Account"
        }
      ]
    }
  ]
}
```

The config above generates the form `Account` (which is of type `Main`). As you can see the `forms` property is an array. This allows you to generate multiple form object for the same entity. The following example shows how to add the contact form to the config:

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
      "generateOdata": true,
      "forms": [
        {
          "formType": "Main",
          "formName": "Contact"
        }
      ] 
    }
  ]
}
```

After modifying the config simple execute the `GenerateFrontend.ps1` powershell script.