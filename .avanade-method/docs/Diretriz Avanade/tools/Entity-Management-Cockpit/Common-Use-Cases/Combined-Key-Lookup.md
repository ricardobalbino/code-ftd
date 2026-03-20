When looking for a specific record with multiple attributes, you can use the pipe symbol to find the record you want to update:

```
<entity name="account" sheet="Accounts" key="name|ava_sapnumber" keycolumn="1|2" startrow="2">
  <columns>
    <column name="name" column="1" />
    <column name=" ava_sapnumber " column="2" />
  </columns>
</entity>
```

Using the pipe "|" symbol to combine two different key and keycolumn
values provides the ability to find a specific record if there is no
unique identifier available in a single field.

Here the example is searching for an account that can be identified by
the combination of name and ava_sapnumber.

In a similar way you can find a lookup record for your current record
with piped identifiers.
This is different than the example above in N:N relationship, as you do
not need to use the refentity attribute in the xml for the alias
columns:

```
<entity name="account" sheet="Accounts" key="name" keycolumn="1" startrow="2">
    <columns>
      <column name="name" column="1" />
      <column name="ava_branchmanagedaccountid" column="2"  refentity="ava_branch" refkey="ava_description|ava_branchnumber" refcolumn="ignore_ava_description|ignore_ava_branchnumber" />
      <column name="ignore_ava_description" column="2" refkey="ava_description" ignore="true" />
      <column name="ignore_ava_branchnumber" column="3" refkey="ava_branchnumber" ignore="true" />  
  </columns>
</entity>
```

Here the main record is Account, while the lookup field
"ava_branchmanagedaccountid" should be filled with a Branch that is
identified by the two fields "ava_description|ava_branchnumber".

To get the right Branch, the two below columns (2 and 3) are used with
the ignore="true" attribute and pointing to the field with the refkey
attribute. The name attributes of these alias columns are used in the
refcolumn values of your lookup column above.

Notice the missing refentity tag (which was used in the N:N example
above). If used, the resulting LinkEntity would look for an entity
record of that type instead of a field value.