The main use case for this functionality is that when you want to import or export multiple CSV files
(typically to store the data in a repository and have better diffs of the data changes) and you don't
want to configure every action for each CSV file individually.

It works as follows:

- Set up your configuration XML as usual with all entities included in one XML file. (Typically, you can use the same XML for import and export.)
- Instead of providing a single file in the `importfile` location, provide a folder location.
- **Import:** All files in the folder are imported, the match between the CSV files and `<entity>` configuration is done via the sheet name (without considering the ".csv" extension).
- **Export:** The export files are named as by the sheet names configured in the `<entity>` nodes suffixed with the ".csv" extension.