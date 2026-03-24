In order to merge two entity records, the type "merge" can be used in combination with a "subordinateid" referencing the record to be merged. The record to be merged into (target) is simply the main record associated with each Excel row (identified by the column key(s) like for any create or update configuration).

**Notes:**

-   The underlying MergeRequest uses
    *PerformParentingChecks* as false.

-   Furthermore, **empty target entity fields are updated** the with corresponding field values from subordinate entity if those fields are not empty.

```
<!-- Example: Merge Accounts (by name) -->
<entity name="account" sheet="Account Merge" key="name" keycolumn="1" startrow="2">
  <columns>
    <column name="name" column="1" />
    <column name="subordinateid" column="2" refentity="account" refkey="name" type="merge" />
  </columns>
</entity>

<!-- Example: Merge Accounts (by ID)-->
<entity name="account" sheet="Account Merge" key="accountid" keycolumn="1" startrow="2">
  <columns>
    <column name="accountid" type="guid" column="1" />
    <column name="subordinateid" column="2" refentity="account" refkey="accountid" type="merge" />
  </columns>
</entity>
```
