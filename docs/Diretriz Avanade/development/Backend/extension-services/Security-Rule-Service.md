# Security Rule Service

This service provides a full-fledged engine to assign owner teams or set the owning business unit (modernize business units feature introduced in 2022) of Dataverse records based on Fetch XML-based rules or the parent record.

## Typical Use Case

The service helps with the implementation of the Security Automation concept described [here](https://adf.avanade.com/display/FODG/%28CRM%29+Security+Automation).

## Considerations

The service can set the owner team or the owning business unit based on the `SecurityAssignType` property of the `SecurityRule` object.

```csharp
public enum SecurityAssignTypeValue
{
   OwnerTeam,
   OwningBusinessUnit
}
```

Setting the owner team was the way to handle record ownership before the modernize business unit was introduced in 2022. The latter is the way to go according also to Microsoft, among other benefits, it simplifies the security concept as less configuration data overhead is needed.

!!! tip Enable modernize business unit feature
    In order to make sure that you can set the owning business on a record, you need to enable the feature as described [here](https://docs.microsoft.com/en-us/power-platform/admin/update-record-owner).

## Methods

`void ProcessSecurityRules(Entity entity, ISecurityRuleRepository securityRuleRepository)`

Processes the Security Rules for a given record. Note that a Security Rule Table is not included on purpose to allow project or asset teams to define their own data model, only `ISecurityRuleRepository` must be implemented to map the Table columns to the `SecurityRule` object required to process the rules.

## Example

### Fetch XML

A Fetch XML rule which defines that a certain owner of a case shall be set based on the country of the related account, looks as follows:

```xml
<fetch version=""1.0"" output-format=""xml-platform"" mapping=""logical"" distinct=""false"">
  <entity name=""incident"">
    <attribute name=""title"" />
    <order attribute=""title"" descending=""false"" />
    <link-entity name=""account"" from=""customerid"" to=""accountid"" link-type=""inner"" alias=""aa"">
      <filter type=""and"">
        <condition attribute=""ava_countryid"" operator=""in"">
          <value uiname=""DE"" uitype=""ava_country"">{8091f3ad-ce27-4e29-8b3e-56f0153fbf0b}</value>
        </condition>
      </filter>
    </link-entity>
  </entity>
</fetch>
```

### ISecurityRuleRepository

The repository needs to be implemented in order to map your project's or asset's specific Security Rule data model to the class object required for the Security Engine to process the rules. Thi is typically a very simple task as shown in the example below.

Please note that you do not need to worry about the caching of the rules, this is handled by the `SecurityRuleService` automatically for high performance processing and for your convenience.

```csharp
public class MySecurityRoleRepository : ISecurityRuleRepository
{
   ...

   public IEnumerable<SecurityRule> GetSecurityRules(string entityLogicalName)
   {
      List<SecurityRule> securityRules = new List<SecurityRule>();

      foreach (MySecurityRule mySecurityRule in _mySecurityRuleRepository.GetAll().Where(m => m.TableName.Equals(entityLogicalName))
      {
         if (mySecurityRule.Type == Inherit)
         {
            securityRules.Add(new SecurityRuleInherit
            {
               Name = mySecurityRule.Name,
               ...
            });
         }
         else
         {
            securityRules.Add(new SecurityRuleFetchXml
            {
               Name = mySecurityRule.Name,
               ...
            });
         }
      }

      return securityRules;
   }
}
```

### Case Plug-In

```csharp
...
_securityRuleService.ProcessSecurityRules(target, _mySecurityRoleRepository);
...
```
