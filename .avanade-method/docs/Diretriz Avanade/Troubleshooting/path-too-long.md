# Path too long causing Projects to fail loading in Solution

Some users are running into issues with a too long file path which results in a problem with loading all projects in the DSS Visual Studio Solution.

To fix this until the root cause is solved, a hard link can be used.

- Open PowerShell as Admin
- Open the root folder of your DSS Solution in the PowerShell console
- Type: `subst x: .` **Note:** If you are already using `x:` as a drive, just use a different character
- Open the Solution from Drive `x:`