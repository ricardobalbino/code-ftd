It is possible to import Saved Views or Advanced Find queries:

-   On the sheet containing the Advanced Finds, add Fetch XML expressions as exported from any system/environment including GUIDs. If you do not provide fetch XML expressions, blank queries are created instead.

-   You can optionally use Excel to build these expressions dynamically, for example if a couple of similar Saved Views with only different conditions have to be created.

-   **Important:** During import, the GUIDs contained in the "value" attributes of the  fetch XML expressions are replaced **dynamically** according to the entity records available in the destination system. If a record is not present in the destination system, an error is logged.
