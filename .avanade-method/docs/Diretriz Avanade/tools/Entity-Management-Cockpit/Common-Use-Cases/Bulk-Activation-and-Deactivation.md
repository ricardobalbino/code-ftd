# Bulk Activation/Deactivation of Users

You just need a two-column Excel sheet where you have to fill in the
domain name of the users and set "Is Disabled" (type "twooptions") to
the respective value ("Disabled" or "Enabled"):

```
<entity name="systemuser" sheet="User" key="domainname" keycolumn="1" startrow="2">
   <columns>
    <column name="domainname" column="1" />
    <column name="isdisabled" column="2" type="twooptions" />
  </columns>
</entity>
```

  User Name / Domain Name   | Is Disabled
  ------------------------- | -------------
  `user1@abc.com`         | Disabled
  `user2@abc.com`         | Enabled

# Bulk Activation/Deactivation of Other Entities

Changing the status of a record should not be a common configuration
scenario but needful when it is required to update the status of a bulk
of records when not possible via the bulk edit functionality on the UI.
In such a case, a one-time configuration will work:

For all entities except for users two configurations with the column
names "statecode" and "statuscode" need to be added. In the
corresponding Excel column the English display names of the respective
labels need to be given. For most common entities these are
"Active"/"Active" to activate records or "Inactive"/"Inactive" to
deactivate records. For example:

```
<entity name="new_custom" sheet="Custom" key="new_name" keycolumn="1" startrow="2">
  <columns>
    <column name="new_name" column="1" />
    <column name="statecode" column="2" />
    <column name="statuscode" column="3" />
  </columns>
</entity>
```