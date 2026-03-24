# Fetch XML Service

This service is very helpful when a set of rules has to be efficiently evaluated against one or multiple Table rows (or Entity records) and if those rules can be formulated as Fetch XML. Fetch XML has been proven as a very flexible approach because it can be exported from Advanced Finds and Power Users find it understandable and relatively easy to maintain if needed. It is also a less proprietary format than many others.

The rule evaluation is done efficiently in memory by comparing the given record with the parsed Fetch XML.

> **Note:** If needed, the trade-off of having to maintain the Fetch XML via copy and paste in a multi-line text field can be compensated at some point by created a PCF component (ideally in the Avanade PCF Library) to handle Fetch XML expressions.

# Typical Use Cases

* Used in the Security Rule Service (part of the Extension Services) to determine the owning business unit or owner team of a record
* Used in a Notification Engine to check if a certain Notification Configuration is relevant for a record
* Custom Routing Engine (if the standard mechanisms are not sufficient)

# Methods

`QueryExpression GetQueryExpressionForFetchXml(string domain, EntityReference entityReference, string fetchXml, List<string> columns = null)`

Converts a Fetch XML expression to Query Expression and caches the result by the given domain (for example to use the FetchXmlService for both Security Rules and Notification Rules). The logic traverses through all filters, conditions and linked entities and adds all columns so that the record retrieved with the expression can be effectively compared with the Fetch XML expressions in memory.
        
`bool IsFetchXmlApplicableForEntityRecord(Entity entity, string fetchXml, QueryExpression queryExpression)`

Determines whether the given entity record matches the Fetch XML expression. The method expects that the provided entity record already contains all columns and aliased columns retrieved with the matching QueryExpression and that the QueryExpression matches with the Fetch XML. This method is typically used in an advanced scenario where for examples entity records have been retrieved in bulk for performance optimizations or similar.
        
`bool IsFetchXmlApplicableForEntityRecord(string domain, EntityReference entityReference, string fetchXml)`     

**Note:** This method must be used **only** when the record is checked against **one single** Fetch XML because even though it caches the converted QueryExpression, the method retrieves the record every time, so more multiple rule evaluations, it would retrieve the same record multiple times.
        
# Example

In order to compare a given record with multiple Fetch XML rules, implement the following logic in a service class which has `IOrganizationService` and `IFetchXmlService` injected:

```csharp
// The Fetch XML strings are typically stored in a custom Table, let's call it "CustomRule" here.
// The record to be checked must be available as an "entityReference".
foreach (CustomRule rule in rules) {
  // This is the most effective way but it assumes that the Fetch XML expressions in all rules are similar (same columns and linked entity conditions to compare). If not, then you need to retrieve the record for every rule which can be costly.
  QueryExpression query = _fetchXmlService.GetQueryExpressionForFetchXml("CustomRule", entityReference, rule.FetchXml, new List<string> { "ava_mycolumn" });
  
  if (cachedRecord == null)
  {
    cachedRecord = _organizationService.RetrieveMultiple(query).Entities.FirstOrDefault();
  }
  
  if (_fetchXmlService.IsFetchXmlApplicableForEntityRecord(cachedRecord, rule.FetchXml, query))
  {
     // The rule is applicable, do something.
  }
}
``` 
