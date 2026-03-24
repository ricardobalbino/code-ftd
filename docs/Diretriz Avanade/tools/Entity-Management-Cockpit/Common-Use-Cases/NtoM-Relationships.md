N:M relationships can be imported with the following configuration:

-   Configure the <entity> node without the "key" attribute and with
    the name of the relationship as the entity name

-   **First** <column> node: From entity (N)

-   **Second** <column> node: To entity (M)

-   Other <column> nodes can be used to provide combine key lookups
    over multiple columns.

> **Note:** If combined keys are used, the order of the columns in XML is important!
The first 2 columns must contain the real ID-columns (e.g. accountid and
ava_brandid in Example 2). The Schema Name (NOT the Logical Name) of the relationship needs to be
used for import and export (Approach works case sensitive due to
behavior of internal Dataverse API Requests).

**Example - Basic mapping:**

```
<entity name="new_new_custom1_new_custom2" sheet="Relationship" keycolumn="1" startrow="2">
   <columns>
      <column name="new_custom1id" column="2" refentity="new_custom1" refkey="new_name" />
      <column name=" new_custom2id" column="3" refentity="new_custom2" refkey="new_name" />
   </columns>
</entity>
```

**Example 1 - Combined keys mapping:**

```
<entity name="new_new_custom1_new_custom2" sheet="Relationship" keycolumn="1" startrow="2">
   <columns>
      <column name="new_custom1id" column="2" refentity="new_custom1" refkey="new_name" />
      <column name="new_custom2id" column="3" refentity="new_custom2" refkey="new_name|new_lookupcustom3" refcolumn="new_custom2 new_custom3_ignorecolumn" />
      <column name="new_custom3_ignorecolumn" column="1" refentity="new_custom3" refkey="new_name" ignore="true" />
   </columns>
</entity>
```

**Example 2 - Combined keys mapping:**

```
<!-- Brand-Account relation via Postalcode and Street -->
<entity name="ava_BrandsSoldByAccount" sheet="Account-Brand" keycolumn="1" startrow="2">
   <columns>
      <column name="accountid" column="4" refentity="account" refkey="address1_line1|address1_postalcode" refcolumn="address1_line1_rel|address1_postalcode_rel" />
      <column name="ava_brandid" column="3" refentity="ava_brand" refkey="ava_name" />
      <column name="address1_line1_rel" column="1" ignore="true" />
      <column name="address1_postalcode_rel" column="2" ignore="true" />
   </columns>
</entity>
```

Account Street  |  Account Zip Code |   Brand
--- | --- | ---
  Maxstraße 1    |  97777         |     Brand 1
  Herzstr. 2     |  70000       |       Brand 2
  Petersplatz 5   | 70111      |        Brand 1
  Frankweg 9     |  78000       |       Brand 3
  Zepfplatz 88  |   98989      |        Brand 4
