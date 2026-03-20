# Metadata Service

This service allows quick access to selected metadata via caching.

# Typical Use Cases

- All scenarios where metadata is required in the backend.

# Methods

`string GetPrimaryIdAttribute(string entityLogicalName)`
`string GetPrimaryNameAttribute(string entityLogicalName)`

Retrieves cached primary ID or name of a Table.

`EntityFieldMetadata GetEntityFieldMetadata(string entityLogicalName, string attributeLogicalName)`

Retrieves all attribute metadata of a table column wrapped into a `EntityFieldMetadata` class for quick access to the main properties.

`bool IsDateOnly(AttributeMetadata attributeMetadata)`
`bool IsTimezone(AttributeMetadata attributeMetadata)`

# Examples

The usage of this service is straight-forward.