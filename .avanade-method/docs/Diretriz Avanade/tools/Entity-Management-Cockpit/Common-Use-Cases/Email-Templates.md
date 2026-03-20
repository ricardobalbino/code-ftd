You can use the following schema XML to import and also export Email Templates to and from any environment. 

```
<entity name="template" sheet="Email Templates" key="title" keycolumn="2" startrow="2">
  <columns>
    <column name="title" column="2" />
    <column name="languagecode" column="3" type="languagecode" />
    <column name="templatetypecode" column="4" />
    <column name="description" column="5" />
    <column name="subject" column="6" />
    <column name="body" column="7" />
    <column name="subjectpresentationxml" column="8" />
    <column name="presentationxml" column="9" />
  </columns>
</entity>
```