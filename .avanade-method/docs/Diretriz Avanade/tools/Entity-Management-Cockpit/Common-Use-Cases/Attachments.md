Attaching a file to an `annotation` (Note) entity recording including a
relation to another entity (e.g. Case) works with the following configuration:

```
<entity name="annotation" sheet="Attachment Files" key="subject" keycolumn="3" startrow="2">
<columns>
    <column name="changetype" column="1" type="changetype" />
    <column name="path" column="2" type="path" />
    <column name="subject" column="3" />
    <column name="objectid" column="4" refentity="incident" refkey="title" />
  </columns>
</entity>
```