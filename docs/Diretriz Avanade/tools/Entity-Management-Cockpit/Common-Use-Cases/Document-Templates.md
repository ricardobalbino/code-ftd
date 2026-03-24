To import/export Document Templates, the following syntax can be used.

```
<!-- Documenttemplates -->
<entity name="documenttemplate" sheet="Document-Template" key="documenttemplateid" keycolumn="1" startrow="3">
  <columns>
    <column name="documenttemplateid" column="1" type="guid" />
    <column name="name" column="2" />
    <column name="clientdata" column="3" />
    <column name="documenttype" column="4" type="optionset" />
    <column name="associatedentitytypecode" column="5" />
    <column name="languagecode" column="6" type="wholenumber" />
    <column name="content" column="7" type="path" />
  </columns>
</entity>
```

**Note:**

For the import, the following shortcuts can be used in the Excel sheet
cells:

`{CurrentDirectory}` to access the execution path of CRMEMC application.

`{XmlConfigurationDirectory}` to access the path of the XML
configuration file. This is for example helpful if you store your
deployment package including the XML configuration in a different folder
structure then the tool itself.

Client Data (From Export) | Type  | Associated Entity Type Code |   Language |  Path
--- | --- | --- | --- | ---
  {&quot;Range&quot;:null,&quot;Columns&quot;:null,&quot;DisplayConditions&quot;:&quot;u003cDisplayConditionsu003eu003cEveryone /u003eu003c/DisplayConditionsu003e&quot;}   | Microsoft Word |     incident                      | 1033       |  C:\Templatesyourfile.docx
  {&quot;Range&quot;:null,&quot;Columns&quot;:null,&quot;DisplayConditions&quot;:&quot;u003cDisplayConditionsu003eu003cEveryone /u003eu003c/DisplayConditionsu003e&quot;}   | Microsoft Excel   | account                      |  1031   |     {CurrentFolder}\Templatesyourfile.xlsx
  {&quot;Range&quot;:null,&quot;Columns&quot;:null,&quot;DisplayConditions&quot;:&quot;u003cDisplayConditionsu003eu003cEveryone /u003eu003c/DisplayConditionsu003e&quot;} |   Microsoft Word    | incident                     |  1033       | {XmlConfigurationDirectory}\\..\DocumentTemplatesService Order.docx
