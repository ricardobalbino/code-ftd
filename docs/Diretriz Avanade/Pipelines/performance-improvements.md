# Performance Improvements

The BizApps Core Accelerator tries to keep a balance between high performance on one side and high extensibility, flexibility and robustness on the other. The latter sometimes impact performance because for example additional quality checks are being run which leads to a higher pipeline execution time.


This section provides some hints how to boost your deployments.

???+ info "Feedback highly welcome"
    The team is constantly trying to improve performance and welcomes improvement suggestions very much.

## Upsize Build Infrastructure

If you are using [Microsoft-hosted agents](https://docs.microsoft.com/en-us/azure/devops/pipelines/agents/agents?view=azure-devops&tabs=browser#microsoft-hosted-agents) you can potentially switch to [self-hosted agents](https://docs.microsoft.com/en-us/azure/devops/pipelines/agents/agents?view=azure-devops&tabs=browser#install) running on a reasonable sized infrastructure/VM (see [here](./Self-Hosted-Agent-Setup.md) for more information).

## Increase Parallel Jobs

Sometimes it can help to increase the number of [parallel jobs](https://docs.microsoft.com/en-us/azure/devops/pipelines/licensing/concurrent-jobs?view=azure-devops&tabs=ms-hosted) in Azure DevOps. This at least allows multiple guards builds to run in parallel to each other and also in parallel to 

???+ info
    Please note that when running a self-hosted infrastructure that you need multiple agents installed on the VM to run parallel jobs.

## Skip Test Execution

The tests for backend and frontend are by default executed during the [Guard Build](./dataverse-build-guard.md) as well as during the [Deploy](./dataverse-deploy.md) and [Deploy Code](./dataverse-deploy-code.md) pipelines. This double (or triple) check adds an extra layer of quality to safeguard against the unlikely but possible chance that guard build policies are circumvented and invalid code is committed.

In case your Guard Builds are in place and you are sure that after the commit to the main branch code tempering is avoided, you can skip the test execution to reduce pipeline execution time by up to several minutes:

1. Go to `cicd/templates/stages/dataverse-build.yaml`.
1. Add the `test` parameter to the `dataverse-build-backend.yaml` template call like this:
```yaml
- template: ../steps/build/dataverse-build-backend.yaml
  parameters:
    test: false
```
1. Add the `test` parameter to the `dataverse-build-frontend.yaml` template call like this:
```yaml
- template: ../steps/build/dataverse-build-frontend.yaml
  parameters:
    test: false
```
