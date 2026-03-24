# Deploy Pipeline

The `dataverse-deploy` pipeline is used to build the backend and frontend artifacts (non lightweight projects), build the Dataverse solutions (including the backend and frontend artifacts - non lightweight projects) and imports the solutions into the environments, performs pre and post deployment automation steps as well as the relevant data imports. 

## Details

The pipeline executes the following steps:

- **dataverse-build** (non lightweight): Builds frontend and backend, executes tests and publishes the artifacts.
- **dataverse-prepare-solutions**: Builds the Dataverse solutions (.cdsproj) (replaces the frontend and backend artifacts - non lightweight). It also increases the version of all solutions which have been selected as queue time parameters in the Customization Master.
- for each environment (Dev-Int, Dev-Test, UAT, Prod):
    - **deploy-generic**:
        - **initialize-environment**: Import all managed solutions stored in the repository (e.g. the BCA Core solution) and run pre steps from `PreSteps.xml`.
        - **initialize-environment**: Import all managed solutions stored in the repository (e.g. the BCA Core solution) and run pre steps from `PreSteps.xml`.
        - **import-all-solutions**: Imports all Dataverse solutions which have been built and prepared above.
        - **dpa-import-data**: Imports configuration data, test data (if enabled for the environment) and other data if configured.
        - **dpa-execute-post-steps**: Execute post steps from `PostSteps.xml`.
        - **publish-changes**: Runs Publish All if running unmanaged.



