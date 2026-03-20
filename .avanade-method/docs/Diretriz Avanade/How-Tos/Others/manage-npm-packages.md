# Manage npm Packages

## Background

BizApps Core Accelerator embraces `Node.js` for frontend development which utilizes `npm` as the underlying package manager. This is aligned with Microsoft's way of creating [PCF Components](https://docs.microsoft.com/en-us/powerapps/developer/component-framework/overview) as they want to align with current web development standards.

A consequence of this is that you might need to deal with the occasional package management tasks as part of your project if you are developing frontend code or PCF components. This is similar to any regular web development project (most likely less time consuming) which is related to the fact that package versions are getting updated frequently and breaking changes are introduced and sometimes security issues are found and need to be fixed by apply a new package version.

## General Principle

You should regularly run `npm outdated` to see if there are new package versions and then `npm update` to update packages in the frontend (`Web`) project or in any of your PCF components. Please note that a normal update does not upgrade to a major version which is sometimes necessary (see below).

## Package Update in Web Project

In the `Web` project, it should be sufficient to upgrade the `@avanade/bizapps-sdk` which will successively update dependent packages.

## Package PCF Components

Depending on the packages you are using in your PCF components (make sure that you have clearance for external libraries and the underlying licenses), you need to update those.

## Audit Findings and Fixes

If you do not follow this principle, you might see `npm audit` findings during a local `npm` build or when a pipeline is running. This indicates that you should do a package upgrade.

If a regular update does not work, you might need to move to a new major version which using tools and techniques explained for example [here](https://www.carlrippon.com/upgrading-npm-dependencies/).

If a major version upgrade does not work for you or if you directly want to fix the npm findings, you can run `npm audit fix` as suggested by the `npm` build.

## Elevated update experience

In addition to `npm outdated` there is another tool called [ncu](https://www.npmjs.com/package/npm-check-updates). This tool provides a more user friendly update experience when dealing with npm packages.

To install `ncu` run the following command:
```cmd
npm install -g npm-check-updates
```

After `ncu` was installed successfully you can simply type in `ncu` in your CMD or PowerShell and hit return. You will get the same output as `npm outdated` but in a more readable format.

Using `ncu` you can select packages which should be updated in an interactive way by running `ncu -i`. This will instruct `ncu` to interactively prompt for each package if it should be updated or not.

You can find more switches and options on the ncu npmjs page above.