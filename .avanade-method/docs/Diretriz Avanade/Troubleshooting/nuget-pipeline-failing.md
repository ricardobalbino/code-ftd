# Pipeline Failing with NuGet Command Failure

## Error
When running the guard build or deployment pipeline, a build task error is thrown:

`The nuget command failed with exit code(1) and error(Unable to load the service index for source https://innersource.pkgs.visualstudio.com/...`

and 

`Response status code does not indicate success: 401 (Unauthorized)`.

## Root Cause
The Personal Access Token (PAT) for the BCA Azure DevOps Artifact has expired and needs to be renewed and updated in the build variable `INNERSOURCE_PAT`. 

## Solution
1. Create a PAT using an account which has the necessary privileges to access https://dev.azure.com/innersource/DSS-Framework/_artifacts/feed/DSS.
1. Update your build variable `INNERSOURCE_PAT` with the new PAT.