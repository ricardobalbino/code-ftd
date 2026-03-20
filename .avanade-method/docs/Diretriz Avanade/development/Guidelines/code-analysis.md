# Code Analysis

### Static Code Analysis (Minimum)

- The standard Code Analysis of Microsoft Visual Studio and Microsoft Visual Studio Code is used.

- For all documents, the standard rule set "Microsoft Managed Recommended Rules" is used as a starting point.

- If it is found during the project that certain rules need to be removed or added, the rule set will be adapted.

- If for certain parts of the code, the code analysis will have to be suppressed, this will be agreed with the architects and done on a per case basis.

### Static Code Analysis (Recommended)

It is recommended to use StyleCop which provided additional Code analysis rules on top of the Microsoft standard rules. These primarily help to reduce manual code review efforts by detecting specific formatting issues (for example redundant lines of code) etc.

### Dynamic Code Analysis

Dynamic code analysis can be done via e.g. SonarCloud/-Qube.