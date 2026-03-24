# Azure Active Directory Token Service

Requests an access token for an Azure resource using a service principal using versions 1.0 or 2.0.

# Typical Use Cases

- When a general backend call to an Azure Function must be made.
- When a temporary bearer token has to be produced in the backend to be used securely in the frontend via Javascript/PCF.

# Methods

`string RequestTokenV1(string azureActiveDirectoryTenantId, string azureResourceId, string clientId, string clientSecret);`

`string RequestTokenV2(string azureActiveDirectoryTenantId, string scope, string clientId, string clientSecret)`

The main difference between V1 and V2 is that the latter uses the scope to authorize access.

# Examples

```csharp
AzureAccessToken token = azureActiveDirectoryTokenService.RequestTokenV2("myTenantId", "myScope", "myClientId", "myClientSecret");
```
