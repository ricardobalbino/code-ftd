# Deployment Automation

In addition to the automated deployment of Dataverse solutions with their customizations and code and the automated data import, Dataverse projects should also automate other steps to avoid manual work. The **BizApps Core Accelerator** fully supports this with a larger library of pre-configured deployment steps and can extend those to other Dataverse areas as long as the Dataverse API allows for such automation.

The deployment automation is split into [Pre Steps and Post Steps](../../How-Tos/automate-pre-post-steps.md) which can maintained in the respective files `PreSteps.xml` and `PostSteps.xml` in `src/Dataverse/Deployment/DPA`.

The deployment automation can be modified and quickly tested in lower environments as described [here](../../How-Tos/automate-pre-post-steps.md).

Both XML files contain a lot of examples with explanations, further scenarios are described in the [Deployment Tool Wiki](../../tools/Deployment-Tool/index.md).
