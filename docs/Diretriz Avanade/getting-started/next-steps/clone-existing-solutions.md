# Clone Existing Dataverse Solutions

The **BizApps Core Accelerator** recommends creating Dataverse solutions from scratch through `bizapps-cli.ps1` (this will create empty solutions in the Customization Master etc.) as described [here](../first-steps/create-solutions.md).

However, BCA also supports **brownfield scenarios** where there are already existing solutions which can be easily wrapped into the BCA structure to rapidly enable automated deployments for those solutions. With this, all the other BCA tools and benefits are also directly available.

This excercise walks you through the cloning of existing Dataverse solutions and their addition to the project repository.

## Pre-requisite
1. Clone the project repository in local machine.
2. Open the project and Build the solution in Visual Studio.
3. Install the required tools mentioned in [installing tools](../first-steps/installing-tools.md).

## Cloning a Dataverse Solution
1. Open Command prompt and navigate to project root folder.
2. Create an authentication profile.
    ``` title="Create Auth Profile"
    pac auth create --url <provide crm instance url> --name <Provide instance name>

    ```
3. List the Solution to make sure the solution is available in the instance.
    ``` title="List all Solutions"
    pac solution list
    ```
4. Navigate to the solutions folder.
    `cd src\Dataverse\Solutions`
5. Clone the solution.
   ``` title="Clone a Solution"
   pac solution clone --name <provide solution name>
   ```
   eg: `pac solution clone --name TestSolution`
6. Post Cloning the soltion, Open file explorer and navigate inside the cloned solution folder        location where `.cdsproj` file exists
7. Open the `.cdsproj` file.
8. Add the below line under PropertyGroup as shown in the image.
   ``` title="In .cdsproj file" hl_lines="2"   
   <PropertyGroup>
      <SolutionPackageType>Both</SolutionPackageType>
   </PropertyGroup>
   ```
9.  Open command prompt inside the cloned solution folder where `.cdsproj` file exists.
10. Build the project.
    ``` title="Build the Project"
    dotnet build -c Release
    ```
11. Validate the Build Success.
12. Repeate the step from 5 to 10 if more than one solution needs to be cloned.

## Configurations

Once the solutions are cloned and build sucessfully, you can start configuring it in BCA.

1. Add an entry of the solution created to `Configuration.json`:
    ``` title="Configuration.Json" hl_lines="2 3 4 5 6 7 8 9 10 11"
        "Solutions": [ 
            {
                "SolutionName": "TestSolution",
                "Type": "Build",
                "Webresources": [
                    "entity/Account/Ribbon/account.ribbon.contract.js"
                ],
                "Plugins": [
                    "ExampleSolution.dll"
                ]
            }
        ]
    ```
2. Add Parameters and variables to the `.yaml` files:

    ``` title="dataverse-deploy.yaml" hl_lines="2 3 4 5 12 13"
         parameters:
         - name: "deploy_TestSolution"
           displayName: "Deploy TestSolution"
           type: boolean
           default: true
         - name: "upgrade_solutions"
           displayName: "Upgrade solutions"
           type: boolean
           default: false
        
         variables:
         - name: "deploy_TestSolution"
           value: ${{ parameters.deploy_TestSolution }}
         - template: variables/default.yaml
    ```
    
    ``` title="dataverse-export-unpack.yaml" hl_lines="2 3 4 5 8 9"
         parameters:
         - name: "deploy_TestSolution"
           displayName: "Export TestSolution"
           type: boolean
           default: true
        
         variables:
         - name: "deploy_TestSolution"
           value: ${{ parameters.deploy_TestSolution }}
         - template: variables/default.yaml
    ```

3. Commit the changes and push it to the Git Repository.
