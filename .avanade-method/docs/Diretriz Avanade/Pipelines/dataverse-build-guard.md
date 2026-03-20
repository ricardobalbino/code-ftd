# Build Guard

The `dataverse-build-guard` pipeline is used as the typical CI trigger pipeline, meaning it will be run with every git checkin. It validates that the frontend and backend can be built and all unit tests are passing.

## Details

The pipeline executes the following steps:

- **dataverse-build-backend**: Builds the C# Backend Code and executes the tests
- **dataverse-build-frontend**: Builds the JavaScript Frontend Code and executes the tests

