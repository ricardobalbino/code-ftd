# Environment Approvals

Environment approvals are a [standard concept in Azure DevOps](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/approvals) and allow for certain constraints to be checked - mainly manual approvals given by one or several users or group - before the deployment into an environment can commence.

As of BCA version 3.0, environment approvals are not automatically configured during the setup which means that this should be done as required by the project needs.

The minimum recommendation is the following:

* **Approvers**: Use one Azure DevOps group for all upper environments except Dev-Int (i.e. Dev-Test, UAT, Prod at least), for example `"Approvers <Environment Name>"`. It is usually sufficient to get approval from one user out of this group to do the deployment.

* **Exclusive Lock**: The exclusive lock check allows only a single run from the pipeline to proceed. This avoids potential collisions of several solutions being imported at the same time and can also speed up deployments.

* **Branch control**: For all upper environments except for potentially Dev-Int (i.e. Dev-Test, UAT, Prod at least), set `"refs/heads/main"` as the allowed branch.

For Prod you can additionally use:

* **Business hours**: Setting a respective time window allowed for deployments, it is easily avoided to execute deployments while users are active on the system.