# Create Solutions

The **BizApps Core Accelerator** completely supports (and kind of enforces) a solid solution segmentation and planning approach as it is important to think of your solution structure before starting a project.

Whereas there are many ways to slice your solutions, it is relatively common in a project to first define a base solution holding components (like certain Account columns or forms) which are already planned to be shared or might be shared by multiple solutions in the future. Having a base solution simplifies the onboarding and deployment of other apps to the same environment or even development stream in the future. On top of the base solution, you could have one or several solutions which are split by functionality (like one solution for Case Management for example).

This getting started excercise therefore walks you through the creation of a base solution and a functional solution on top.

??? note "Alternative Solution Segmentation"
    As mentioned above, there are also other ways to slice solutions (technical slicing, solutions for horizontal and vertical assets). See [DevOps Process Documentation](https://adf.avanade.com/display/FODG/%28CRM%29+Tooling+and+DevOps) chapter 4.9 for further information on solution segmentation and consideration.

## Solution Initialization

1. Create a solution using the display name `"<Project Name> Base"` and the default publisher and following the directions given [here](../../How-Tos/new-solution.md).
1. Create a solution using the display name `"<Project Name> <Functional Area>"` and the default publisher and following the same directions [here](../../How-Tos/new-solution.md).

As a result, you shall see the two solutions in your repository as well as in the Customization Master and you are ready to start customizing.

!!! tip
    If you already have a brownfield, i.e. existing solutions which you want to bring into the BCA, this is a very quick process and described in greater detail [here](../next-steps/clone-existing-solutions.md).

## First Customizing and Deployments

Once the solutions are ready, you can start customizing as described in [this flow](../../How-Tos/modify-deploy-customizations.md).

For example, you can open the `"<Project Name> <Functional Area>"` solution created above in the Power Apps Maker Portal, add the existing Account Table to the solution and create a new column and then follow the steps to unpack the solution locally, create a Pull Request and review the changes in the next environment (Dev-Int usually) after the automated or manually triggered deployment.
