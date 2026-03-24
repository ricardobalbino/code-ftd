In the condition XML of Routing Rule Items or when the attribute *usedynamicexpressions* is set to true in a `<column>` node, dynamic expressions for simple attribute based lookups can be used which are resolved within the cell text before the actual processing. This is useful for example in order to resolve a lookup to a record ID in the target system.

The logic resolves the first record found.

**Syntax**

`{!<logical entity name>[<logical attribute name of lookup
attribute>=<lookup value>]:<logical attribute name of selected
attribute>}`

Optionally, a dynamic expression can be followed by .ToUpper() or
.ToLower() for basic string manipulations (e.g. if a GUID needs to be
converted to uppercase).

**Examples**

```
{!account[name=My Customer]:accountid}
{!account[name=My Customer]:name}.ToUpper()
{!account[name=My Customer]:name}.ToLower()
```
