# Automate Pre and Post Steps

Avanade BizApps Core by default provides an extensible framework to add automated deployment steps (executed by the [Deployment Tool (DPA)](../../tools/Deployment-Tool)). As some steps (like setting System Settings in Dataverse) need to be executed before the solution import ("Pre Steps") and some afterwards ("Post Steps"), there are two XML files (`PreSteps.xml` and `PostSteps.xml`) to define the proper steps.

After the initial setup, the automation files are being processed already as part of the automated pipelines but do not perform any tasks. Tasks need to be enabled by commenting in Commands in the XML files or adding further steps using the [example library](https://dev.azure.com/innersource/DSS-Framework/_git/DeploymentTool?path=/DeploymentTool.Console/Xml/Examples&version=GBmaster).

Both XML files come with a lot of examples already and simple instructions in the comments.

In order to avoid trial and error by modifying the XMLs and running pipelines, the CLI supports a very easy mechanism to try configuration in a lower environment which is very quick and painless:

1. Modify `PreSteps.xml` or `PostSteps.xml` in `src/Dataverse/Deployment/DPA`. (Follow the instructions in the XML comments.)
1. Run `bizapps-cli.ps1`.
1. Select `ExecuteDPA`.
1. Select `PreStep.xml` or `PostSteps.xml`.
1. Select the lower environment to run the deployment against.
1. Once you have achieved the desired configuration, create a [pull request](../create-pull-request) and afterwards all your deployment steps will be automatically executed in all upper environments.

??? note "Secret Handling"
    
	For secret handling, please refer [here](Others/add-secrets.md).