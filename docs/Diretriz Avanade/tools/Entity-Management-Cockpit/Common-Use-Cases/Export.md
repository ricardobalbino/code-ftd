It is possible to export Dataverse data into Excel or into single files using
the structure defined in the configuration XML file. This feature can be
used to easier migration data from one system to another.

If the workbook is not provided, a new one will be created.

## General

The export can be triggered via the following command considering the
optional XML configurations for Excel and file exports described below.

```
CRMEMC.App.exe -commandline -export [-configuration:<Configuration XML
from 'Config' folder>] [-workbook:<Path to workbook or CSV to be
exported>] [-connectionstring: "<Connection String>"]
```

-   In general, the XML configuration works in the same way as the configuration for the import with the difference that there can be an additional `<export>` node within each `<entity>` node which specifies the export in greater detail. If no `<export>` node would be contained, then all entity records will be exported as specified in `<entity>`.

-   The export type is defined in the `<exporttarget>` node. Possible values are "excel", "csv" and "file".

-   If the export type is "file", the following additional nodes need to be specified:

    -   `<filename>`:

        -   Naming schema for the export files.

        -   The schema can contain fixed elements as well as reference
            dynamic columns (e.g. "{{1}}" to include the value from
            column 1).

    -   `<filecontent>`:

        -   Columns which contain the actual content to be dumped into
            each file.

        -   Columns and fixed elements can be referenced in the same way
            as the filename.

-   For both export types, there is the possibility to define a `<fetchxml>` node within `<export>` which contains a standard Fetch  XML statement to narrow down the entities to be exported. If `<export>` is not provided, all records are exported.

-   Optionally, there can be a page count defined for each export in order to enable the download of larger attachments in tranches. This parameters is defined in the `<pagecount>` node. The default page count is 1000 and could lead to a timeout in the retrieval of the attachments.

- It is optionally also possible to define the target timezone to convert the exported value to using the .NET timezones, for example `<timezone>W. Europe Standard Time</timezone>`.

## Example: Export to Excel

The following XML specifies an export of all cases between 01.01.2020
and 31.08.2021.

```
<entity name="incident" sheet="Cases" key="title" keycolumn="1" startrow="2">
  <export>
    <fetchxml><![CDATA[<fetch version="1.0" output-format="xml-platform" mapping="logical" distinct="false">
    <entity name="incident">
      <attribute name="title" />
      <attribute name="createdon" />
      <filter type="and">
        <condition attribute="createdon" operator="on-or-after" value="2020-01-01" />
        <condition attribute="createdon" operator="on-or-before" value="2021-08-31" />
      </filter>
    </entity>
    </fetch>]]></fetchxml>
    <exporttarget>excel</exporttarget>
  </export>
  <columns>
    <column name="title" column="1" />
  </columns>
</entity>
```

## Example: Export to File

The following XML specifies the export of all attachments related the
cases which have been created between 01.01.2020 and 31.08.2021. The
files are stored in the "attachments" subfolder. The exported files
names are following the schema:

`<Case Number>_<Annotation Id>_<Filename (including extension)>`

```
<entity name="annotation" sheet="attachments" key="subject" keycolumn="2" startrow="2">
  <export>
    <fetchxml>
      <![CDATA[<fetch version="1.0" output-format="xml-platform" mapping="logical" distinct="false">
      <entity name="annotation">
      <filter type="and">
        <condition attribute="isdocument" operator="eq" value="1" />
      </filter>
      <link-entity name="incident" from="incidentid" to="objectid" alias="ae">
      <filter type="and">
        <condition attribute="createdon" operator="on-or-after" value="2021-01-01" />
        <condition attribute="createdon" operator="on-or-before" value="2022-08-31" />
      </filter>
      </entity>
      </fetch>]]>
    </fetchxml>
    <exporttarget>file</exporttarget>
    <filename>{{1}}_{{2}}_{{3}}</filename>
    <filecontent>{{4}}</filecontent>
    <pagecount>100</pagecount>
  </export>
  <columns>
    <column name="objectid" column="1" refentity="incident" refkey="title" />
    <column name="annotationid" column="2" />
    <column name="filename" column="3" />
    <column name="documentbody" column="4" />
  </columns>
</entity>
```

## Example: Export N:M Relationships

The export of N:M relationships can be defined in the same way as the
import of N:M relationships (see 2.4.1) with the only difference that
the "From Attribute Name" must be given in the column "name" and the
columns to be exported from each linked entity must be defined in
"refkey". This means that if you want to export multiple columns from
the same linked entity, you need to make the "name" unique but still
keep the attribute name information which you can do by adding "-" and
another suffix to make each column unique.

**Example - Basic mapping:**

```
<entity name="new_new_custom1_new_custom2" sheet="Relationship"
keycolumn="1" startrow="2">
  <columns>
    <column name="new_custom1id" column="2" refentity="new_custom1" refkey="new_name" />
    <column name="new_custom2id" column="3" refentity="new_custom2" refkey="new_name" />
  </columns>
</entity>
```

**Example - Multiple columns export:**

```
<entity name="new_new_custom1_new_custom2" sheet="Relationship"
keycolumn="1" startrow="2">
  <columns>
    <column name="new_custom1id" column="2" refentity="new_custom1" refkey="new_name" />
    <column name="new_custom2id-A" column="3" refentity="new_custom2" refkey="new_name" />
    <column name="new_custom2id-B" column="3" refentity="new_custom2" refkey="new_description" />
  </columns>
</entity>
```

## User and Team Export

The export of users and teams works in the same way as for other
entities (i.e. you can seamlessly reuse the configuration which is also
valid for the user/team import). Only some attention has to be paid to
the three special columns of the following types:

-   team

-   securityprofile

-   securityrole

Those columns contain the related records (Roles, Field Level Security
Profiles and Teams) in denormalized columns, therefore there must be as
many columns configured as there are records, otherwise error messages
are logged. For example, if a user has five teams in Dataverse but in the XML
configuration only four columns are configured, there will be an error
that there are not enough columns in the configuration.

This is especially important to consider when the use case is to export
some user configuration to Excel, modify it in bulk (for example a mass
update of Business Units where the tool will ensure that the previous
security roles will be reassigned as Dataverse removes the roles when a
Business Unit is changed) and the reimport it because if the export
misses some items, they cannot be reimported of course.

## Export to CSV

In order to export as CSV, just provide a file export name with the
extension ".csv" as part of the "-workbook" switch, for example:

`-workbook:contacts.csv`
