# Pipelines

The **Avanade BizApps Core Accelerator** uses YAML based CI/CD pipelines. YAML pipelines are no longer configured using the web-interface but live inside the Git repository as .yaml files which implementes the pipeline as code principle.

This enables easier updates to the pipeline, more features than the legacy pipelines and are on a per-branch basis which allows the divergence of different deployment mechanism/tasks (e.g. per release).

A full documentation about the YAML pipelines is provided by Microsoft and can be accessed [here](https://docs.microsoft.com/en-us/azure/devops/pipelines/yaml-schema?view=azure-devops&tabs=schema).

For more information see [here](../../Pipelines/index.md).
