See the example Excel and the corresponding `EntityConfiguration.xml` on how to import and publish/unpublish Duplicate Detection Rules.

```
<!-- Example: Duplicate Detection Rules -->
<entity name="duplicaterule" sheet="Duplicate Detection Rules" key="name" keycolumn="1" startrow="3">
  <columns>
    <column name="name" column="1" />
    <column name="description" column="2" />
    <column name="baseentityname" column="3" />
    <column name="matchingentityname" column="4" />
    <column name="iscasesensitive" type="twooptions" column="5" />    <column name="excludeinactiverecords" type="twooptions" column="6" />  
  </columns>
</entity>

<!-- Example: Duplicate Rule Conditions -->
<entity name="duplicaterulecondition" sheet="Duplicate Rule Conditions" key="duplicateruleconditionid" keycolumn="1" startrow="3">
  <columns>
    <column name="duplicateruleconditionid" type="guid" column="1" />
    <column name="regardingobjectid" refentity="duplicaterule" refkey="name" column="2" />
    <column name="baseattributename" column="3" />
    <column name="matchingattributename" column="4" />
    <column name="ignoreblankvalues" type="twooptions" column="5" />
    <column name="operatorcode" type="optionset" column="6" />
    <column name="operatorparam" type="wholenumber" ignoreblanks="true" column="7" />
  </columns>
</entity> 

<!-- Example: Duplicate Detection Rule (Publish) -->
<entity name="duplicaterule" sheet="Duplicate Detection (Publish)" key="name" keycolumn="1" startrow="3">
  <columns>
    <column name="name" column="1" />
    <column name="statecode" column="2" />
    <column name="statuscode" column="3" />
  </columns>
</entity>
```