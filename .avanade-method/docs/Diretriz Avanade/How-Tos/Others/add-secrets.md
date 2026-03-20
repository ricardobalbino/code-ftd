# Add Secrets

1. Add a Library with a linked Key Vault (this is recommended over and more secure than secret variables) to your Azure DevOps project. Make sure that it follows a certain naming convention like `variables-<environment internal>`, e.g. `variables-devint`.
1. Make sure that your secrets are available in the key vault with same names in all environment key vaults. You can use an approach like in [Azure Core](https://dev.azure.com/innersource/AzureAutomationAccelerator/) to manage your secrets if and when you want to go fully automated.

1. In `deploy-generic.yaml`, add the variable group under `job`:
   
   ```
   variables:
   - group: variables-${{ parameters.internal_name }}
   ```
   This ensures that the environment-specific key vault is always linked to the pipeline executing the deployment to the current target environment.

1. In `dpa-import-data.yaml`, add your secret variables like this:

     `#!pwsh $secureReplacementStrings = @{ "{SecretVariable}" = "$(secret_variable)" }`
     
     In this example, `SecretVariable` is just a placeholder name (see next step how to add it to your configuration data) and the curly brackets {} are only for better replacement, they are just syntax. The `$(secret_variable)` is a secret variable piped from the key vault where a secret must exist under the name "SecretVariable".

2. In any of your .csv files which are imported (Configuration Data, Test Data, etc.), just put the placeholder (in our example `{SecretVariable}`) and then it will be automatically replaced during the deployment.

1. The above steps solve mostly all secret handling requirements but in case you need a secret in a post-deployment step, go to `dpa-execute-post-steps.yaml` and in the call to `Invoke-PostDeploymentSteps`, add the secret under `-GlobalParameters` (in this example `@{ "SecretVariable" = "$(secret_variable)" }`). Then you can read the value in `PostSteps.xml` using `<GetParameterFromJson>` and use it in any Command.