# NPM Unable to Authenticate

## Error
When trying to build web resources via npm an error might occur while authenticating against the npm feed to download the npm packages.

`Unable to authenticate, your authentication token seems to be invalid. To correct this please trying logging in again with: npm login`

## Solution
Please execute the following commands in a CMD/PS shell:

1.	`npm install vsts-npm-auth -g`
2.	Navigate to `src\Dynamics\Web\` in the project's repository.
3.	`vsts-npm-auth -F -config .npmrc`

With the last command a login screen should pop up. Please authenticate with the credentials with which you login into InnerSource.

After that try npm install again. This should pop up a login dialog after some random delay (can take upto mutliple minutes to popup sometimes).

## Background
This typically happens with Accenture IDs.


