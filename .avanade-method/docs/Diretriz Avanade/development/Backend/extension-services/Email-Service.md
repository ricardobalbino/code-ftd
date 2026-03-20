# Email Service

The methods in this service can be used to send emails with or without Email Templates and enrich the rendered text with Dynamic Expressions (which are also part of the Extension Services). The service can also be used to just use the same concepts to render text with dynamics record values which is helpful outside an email context.

# Typical Use Cases

* Send an email with dynamically resolved values from any backend logic (also from Power Automate using Custom APIs or a Command), for example for a Notification Engine
* Render any text using Email Templates, for example to create the header of a custom PDF

# Methods

## SendEmail

These methods send an email using an Email Template provided by Id or Name or without any Email Template. It also replaces Dynamic Expressions (see Dynamic Expressions) for a given source object.

`void SendEmail(string templateName, EntityReference emailRegardingObject, EntityReference sourceObject, EntityReference templateRegardingObject, ActivityPartyWrapper from, List<ActivityPartyWrapper> to, List<ActivityPartyWrapper> cc, List<ActivityPartyWrapper> bcc, Dictionary<string, string> placeholders, EmailWrapper.PriorityCodeValue priorityCode = EmailWrapper.PriorityCodeValue.Normal)`

`void SendEmail(Guid templateId, EntityReference emailRegardingObject, EntityReference sourceObject, EntityReference templateRegardingObject, ActivityPartyWrapper from, List<ActivityPartyWrapper> to, List<ActivityPartyWrapper> cc, List<ActivityPartyWrapper> bcc, Dictionary<string, string> placeholders, EmailWrapper.PriorityCodeValue priorityCode = EmailWrapper.PriorityCodeValue.Normal)`

`void SendEmail(string subject, string description, EntityReference emailRegardingObject, EntityReference sourceObject, ActivityPartyWrapper from, List<ActivityPartyWrapper> to, List<ActivityPartyWrapper> cc, List<ActivityPartyWrapper> bcc, Dictionary<string, string> placeholders, EmailWrapper.PriorityCodeValue priorityCode = EmailWrapper.PriorityCodeValue.Normal)`

## Render Template

These methods renders text/HTML using an Email Template provided by Id or Name or without any Email Template. It also replaces Dynamic Expressions (see Dynamic Expressions) for a given source object. This is very helpful when text with dynamic values is needed anywhere outside emails but using the same template concept.

`SubjectDescription RenderTemplate(string templateName, EntityReference sourceObject, EntityReference templateRegardingObject)`

`SubjectDescription RenderTemplate(Guid templateId, EntityReference sourceObject, EntityReference templateRegardingObject)` 

# Example

Send an email for a given case with a static footer from a shared mailbox (`SharedMailboxQueuedId `):

```csharp
string footer = "This is my footer which is added to all emails."
_emailService.SendEmail(
                "My Case Template",
                incident.ToEntityReference(),
                incident.ToEntityReference(),
                incident.ToEntityReference(),
                new ActivityPartyWrapper { PartyId = new EntityReference(Queue.EntityLogicalName, SharedMailboxQueuedId },
                new List<ActivityPartyWrapper> { new ActivityPartyWrapper { AddressUsed = "to@abc.com" } },
                new List<ActivityPartyWrapper> { new ActivityPartyWrapper { AddressUsed = "cc@abc.com" } },
                new List<ActivityPartyWrapper> { new ActivityPartyWrapper { AddressUsed = "bcc@abc.com" } },
                new Dictionary<string, string> { { "[footer]", footer } },
                EmailWrapper.PriorityCodeValue.Low);
``` 