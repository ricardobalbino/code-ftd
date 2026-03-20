# Dynamic Expression Service

Dynamic Expressions have originated as an extension to the out of the box Merge Fields in Email Template and can be used to dynamically resolve field values to text by overcoming Merge Field limitations:

1. Fields on related Table rows can be reached over multiple levels (for example Case &rarr; Contact &rarr; Account &rarr; Email Address).
1. DateTime values can be converted in a given target timezone overcoming client communication issues which can happen when the user timezone (or timezone independent fields) are used.

# Typical Use Cases

* Include in Email plug-ins or Commands to resolve Dynamic Expressions after inserting an Email Template
* Render any text with dynamically resolved column values

# Methods

## Text Replacements

```csharp
List<string> ReplaceDynamicExpressions(EntityReference sourceEntityReference, List<string> inputTextList)

SubjectDescription ReplaceDynamicExpressions(EntityReference sourceEntityReference, string subject, string description)

string ReplaceDynamicExpression(EntityReference sourceEntityReference, string inputText)
```

These methods replaces Dynamic Expressions in single text expressions, a list of expressions or email subject/descriptions.

## Object Property Resolving

```csharp
void ReplaceDynamicExpressions(EntityReference sourceEntityReference, object o)
```

Using this method, the properties of a given object which are annotated with Dynamic Expressions via the ResolveFromDynamicExpression` attribute.

## Other
```csharp
string GetResolvedExpressionFromGlobalConfiguration(string key, EntityReference entityReference)
```

Replaces Dynamic Expressions stored in a Global Configuration record.

```csharp
string ConvertHtmlToPlainText(string html)
```

Converts an HTML encoded string to plain text (without the need to use HTMLAgilityPack or other packages). 

# Examples

The usage of the abovementioned methods is very straight-forward, therefore only Dynamic Expressions examples are provided and explained in the following.

In general, all Dynamic Expressions for the current record to be evaluated must start with the logical table name and reference the logical names of the columns used to depict the relationships.

|Expression| Purpose | Comment |
|--|--|--|
| `{!incident:contactid:emailaddress;}` | Email address of the contact for the given case | This overcomes the single level limitation. |
| `{!incident:modifiedon;}.ConvertDateTime(-1, "dd.MM.yyyy HH:mm:ss")` | Formats modified on of case consistently regardless of the current user locale | If timezone is provided as -1, the time calculation is skipped |
| `{!incident:modifiedon;}.ConvertDateTime(, "dd.MM.yyyy HH:mm:ss")` | Formats modified on of case consistently regardless of the current user locale | Leaving out the timezone will also skip timezone calculation using -1 as the default value |
| `{!incident:modifiedon;}` | If no convert function is specified for a DateTime field, the formatted value is used | |
| `{!incident:modifiedon;}.ConvertDateTime(2, "dd.MM.yyyy HH:mm:ss")` | Conversion can use a fixed timezone value |  |
| `{!incident:modifiedon;}.ConvertDateTime({!incident:accountid:my_accounttimezone;}, "dd.MM.yyyy HH:mm:ss")` | Conversion can use a dynamically provided timezone field as well |  |
| `{!incident:createdby:utcconversiontimezonecode;}` | Timezones are rendered as string | This also overcomes a typical limitation that Timezones are rendered as a number |
| `Link to Case: <a href='{!incident:recordurl;}'>Link to Case</a>` | Link to a Table row | "recordurl" is a handling directive which creates the deeplink to a Table row overcoming another Email Template limitation and always uses the correct Dataverse instance in the link. |




