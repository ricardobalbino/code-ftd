# Deploy Code Pipeline (non lightweight)

The `dataverse-deploy-code` pipeline is used to build the backend and frontend artifacts, to execute all available unit tests and to deploy backend- and frontend-artifacts to the Customization Master so they can be correlated to a solution.

## Details

The pipeline executes the following steps:

- **dataverse-build**: Builds frontend and backend, executes tests and publishes the artifacts.
- **dataverse-deploy-webresources**: Deploys all frontend code as web resources into Customization Master. It will deploy the artifacts to the solution provided in the `CodeSolutions` node in the `Configuration.json`.
- **dataverse-deploy-plugins**: Deploys the DLLs which contain plugins/workflows into Customization Master using CodeSolutions and perform plugin/-steps/-image registrations.
