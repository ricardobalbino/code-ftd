## Default Values

You can set default values for columns by simply providing the `column` attribute as empty and putting the value in the `value` (alias `defaultvalue`) attribute. With this feature you can for example import subjects with a default feature mask without having to mention the default value redundantly in Excel.

```
<entity name="subject" sheet="Subjects" key="title" keycolumn="1" startrow="2">
  <columns>
    <column name="title" column="1" />
    <column name="featuremask" type="wholenumber" value="1" />
  </columns>
</entity>
```

## Combined / Concatenated Values

You can define concatenated values for columns by simply providing the `column` attribute as empty and putting the references to values in other columns with "{...}" in the `value` attribute. With this feature you can get combined key values in CSV imports where Excel formulas are not possible.

```
<entity name="contact" sheet="Contacts" key="emailaddress" keycolumn="1" startrow="2">
  <columns>
    <column name="firstname" column="1" />
    <column name="lastname" column="1" />
    <column name="emailaddress1" value="{firstname}.{lastname}@example.com" />
  </columns>
</entity>
```
