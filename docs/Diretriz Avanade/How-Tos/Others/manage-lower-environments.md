# Manage Lower Environments

## Transport Solution

Sometimes it might be necessary to bring an unmanaged solution from the repository to the dev environment(s) which can be automatically triggered with this script.

1. Run `bizapps-cli.ps1`.
1. Select `TransportSolution`.
1. Select solution.
1. Select target environment.

## Re-Initialize Lower Environment 

You can fully re-initialize a lower environment which means execute a deployment of all solutions, publisher setup, import of data etc. This is very handy when you need to reset an environment or set up a new one and should only take a few minutes.

1. Run `bizapps-cli.ps1`.
1. Select `ReInitializeLowerEnvironment`.
