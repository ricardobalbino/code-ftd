# Force Create

In order to enforce the creation of a record without checking if it
already exists, the column "Change Type" can be used:

-   Add the following configuration setting to the entity:

`<column name="changetype" column="<column number>" type="changetype" />`

-   In all Excel rows for which a creation shall be enforced, add the keyword "NEW" to in the change type column.

# Force Create for self-referencing entities

In order to enforce the creation of a self-referencing record without checking if its parent already exists, the setting column "ignoreselfreferencing" can be used:

-   Add ignoreselfreferencing configuration setting to the entity:

<entity name="new_custom" sheet="Custom" key="new_name" keycolumn="1" startrow="2" ignoreselfreferencing="true" >

- Warning:  If some parent records are not yet created when there children are to be created, it will fail. Use in this case ignoreselfreferencing="false" or don't set this setting at all.
            Default value of this settting is "false".
