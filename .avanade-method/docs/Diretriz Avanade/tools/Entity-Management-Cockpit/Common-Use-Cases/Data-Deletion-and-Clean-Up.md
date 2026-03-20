Deletion or clean-up of ALL entities in a certain environment might be necessary in certain scenarios and can be achieved under the following conditions.

**Note:**

Be absolutely careful when using this feature because if configured and used in a productive environment it would remove all entity records at once and only a restore of a prior database state would help.

-   The functionality only works via the command-line.

-   The command-line switch "--allowcleanup:true" must be explicitly
    provided.

-   In the *app.config*, the server and environment must be listed in
    "ServersToAllowCleanUp".

-   The following type of configuration must be used (which should not
    be mixed with create/updated actions):

```
<configuration>
  <entity name="new_customentity" id="new_customentityid" key="new_name" action="delete" />
</configuration>
```