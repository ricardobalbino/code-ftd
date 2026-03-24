# Leave Behind

A frequently raised question in project is what happens when Avanade leaves. Since version 3, the "leave-behind scenario" is completely streamlined and automated.

The [Eject Script](../../How-Tos/Others/administer-project-repository.md#execute-eject-script) cuts off all dependencies to the Avanade feed and moves the relevant code for all tools into the project repository.

After this automated step has been taken, the project teams can independently continue and use and extend the Avanade tools on their own.

??? note "Impact"

    Logically, once the eject script has been executed, updates from the Avanade BizApps Core cannot be automatically received anymore.
