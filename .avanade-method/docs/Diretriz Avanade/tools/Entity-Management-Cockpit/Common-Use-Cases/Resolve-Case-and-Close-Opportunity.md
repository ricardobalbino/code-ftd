## Resolve Case/Incident

The resolving of a Case/Incident always goes together with creation of
an Incident Resolution record. This is the record which gets created on
submitting the Dialog which pops up in the UI after clicking "Resolve".
It is not possible to just set the Status and the Status Reason via the
API. The Dataverse Platform expects usage of the "CloseIncidentRequest". This
request contains the case number and expects an Incident Resolution. For
CRMEMC, it has been decided that the trigger for resolving a case is the
creation of the IncidentResolution record.

```
<entity name="incidentresolution" sheet="Incident Resolutions" key="incidentid" keycolumn="1" startrow="2">
  <columns>
    <column name="incidentid" column="1" type="guid" />
    <column name="subject" column="3" />
    <column name="status" column="4" />
  </columns>
</entity>
```

The incidentid must be the GUID of the case which is about to be closed.
The subject is the message of the Incident Resolution (what the user
normally types in the dialog). The Status can be set to 5 (see Status
Reasons of Incident).

  IncidentId          |                   Case Title                     Subject                                   |        Status
  --- | --- | --- | 
  E7362E24-5AB4-E911-8122-0034D8B71DEC |  Test Incident to be resolved  |This is the message used for the Resolve Dialog   |

## Close Opportunity as Won or Lost
In order to close an opportunity as Lost or Won, just set the corresponding [status information](https://docs.microsoft.com/en-us/dynamics365/customerengagement/on-premises/developer/entities/opportunity?view=op-9-1#BKMK_StateCode), the EMC handles the dedicated requests required by the API.
Additional information for the [Opportunity Close](https://docs.microsoft.com/en-us/dynamics365/customerengagement/on-premises/developer/entities/opportunityclose?view=op-9-1) record can be added, such as `actualrevenue` or `description`.

```
<entity name="opportunity" sheet="Opportunities" key="name" keycolumn="1" startrow="2">
  <column name="name" column="1" />
  <column name="statecode" column="2" />
  <column name="statuscode" column="3" />
  <column name="actualrevenue" column="4" />
</entity>
```