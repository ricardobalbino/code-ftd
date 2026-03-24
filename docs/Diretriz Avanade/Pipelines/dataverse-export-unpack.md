# Export Unpack Pipeline

The `dataverse-export-unpack` pipeline automatically exports and unpacks solutions from the Customization Master environment and creates a Pull Request.

!!! tip
    The pipeline can be used optionally in addition or as a replacement to the CLI-based [customization process](../How-Tos/modify-deploy-customizations.md) which requires handling of script and the Git repository) and therefore the pipeline might easy the work and efforts for customizers or Business Analysts.

## Details

The pipeline executes the following steps (only main steps are mentioned):

- **git-checkout-branch**: Checks out the current branch
- **check-empty-solutions**: Checks if at least one solution is defined
- **export-transport-solutions**: Exports and unpacks the solutions
- **git-create-checkout-branch**: Creates a branch `export/<build id>`
- **git-commit-push-solutions**: Pushes the solution changes to branch `export/<build id>`
- **azdo-create-pullrequest**: Creates a Pull Request for branch `export/<build id>` to the main branch
