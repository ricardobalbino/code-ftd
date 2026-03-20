XML File import works very similar to the import of Excel data. Only command-line import is supported.

1.  Instead of providing an Excel file (.xls or .xlsx), provide the XML file with the *-importfile* switch (this switch is an alias for the *-workbook* switch) in the command-line call.

2.  Extend the Entity XML configuration with XPath expressions for each `<entity>` and `<column>` node. This basically instructs the CRM EMC where to take the date for the import. By using XPath, there is a lot of flexibility and you can for example put data for the different entities in the same XML file but in different nodes.

**Example:**

*Entity XML Configuration*
```
<configuration>
  <entity name="contact" sheet="Contacts" key="lastname" keycolumn="1" startrow="2" xpath="data/contacts/rows/row">
    <column name="lastname" column="1" allowblanks="true" xpath="col[position()=1]" />
    <column name="firstname" column="2" allowblanks="true" xpath="col[position()=2]" />
  </entity>
</configuration>
```

*XML Import File*
```
<data>
  <contacts>
    <rows>
      <row>
        <col>Lastname1</col>
        <col>Firstname1</col>
      </row>
      <row>
        <col>Lastname2</col>
        <col>Firstname2</col>
      </row>
    </rows>
  </contacts>
</data>
```
*Command-Line*

`CRMEMC.App.exe -commandline -configuration:EntityConfig.xml
-importfile:"TestDataImport.xml" -connectionstring:"<Connection String>"`