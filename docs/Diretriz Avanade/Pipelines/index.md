# Pipelines

The **Avanade BizApps Core Accelerator** CI/CD pipelines are implemented using YAML. YAML pipelines are not configured using the web-interface but live inside the Git repository as .yaml files which implementes the pipeline as code principle.

This enables easier updates to the pipeline, more features than the legacy pipelines and are on a per-branch basis.

A full documentation about the YAML pipelines is provided by Microsoft and can be accessed [here](https://docs.microsoft.com/en-us/azure/devops/pipelines/yaml-schema?view=azure-devops&tabs=schema).

## Overview

The following main pipelines are included:

- [**dataverse-build-guard**](dataverse-build-guard.md): Builds Frontend and Backend, runs test and makes sure the code works as expected.
- [**dataverse-deploy-code**](dataverse-deploy-code.md): Builds code artifacts and deploys them to Customization Master so they can be correlated to any solution.
- [**dataverse-export-unpack**](dataverse-export-unpack.md): Can be optionally used to automatically export and unpack solutions from the Customization Master environment and create a Pull Request (instead of following the CLI-based [customization process](../How-Tos/modify-deploy-customizations.md) which requires handling of script and the Git repository).
- [**dataverse-deploy**](dataverse-deploy.md): Builds all artifacts and deploys them to the [upper environments](../getting-started/bigger-picture.md#environments).
