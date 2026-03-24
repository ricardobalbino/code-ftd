# Time Zone Service

This service provides multiple functions to easily convert local to UTC times and vice versa and to get time zone information. All methods are using smart caching to reduce overhead.

# Typical Use Cases

- Target times must be calculated based on a time zone stored on another Table than the user
- Timezone label must be displayed

# Methods

`DateTime? ConvertLocalTimeToUtc(int timeZoneCode, DateTime dateTime)`
`DateTime? ConvertUtcTimeToLocal(int timeZoneCode, DateTime dateTime)`

Converts date times for a given time zone.

`DateTime? ConvertLocalTimeToUtc(Guid userId, DateTime dateTime)`
`DateTime? ConvertUtcTimeToLocal(Guid userId, DateTime dateTime)`

Converts date times for the time zone of a given user.

`int GetTimeZoneCode(Guid userId)`

Gets the time zone of a given user.

`TimeSpan GetTimeZoneOffset(Guid userId, DateTime dateTime)`

Calculates the offset between a given user's time zone and UTC.

`GetUserInterfaceName(int timeZoneCode)`

Gets the UI name of a given time zone.

# Examples

Get the UI label of the time zone which is preferred by the given account for communications.

```csharp
_timeZoneService.GetUserInterfaceName(Account.PreferredTimeZone);
```
