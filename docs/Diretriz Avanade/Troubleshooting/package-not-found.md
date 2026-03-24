# NuGet Package not found

## Error
When running the guard build or deployment pipeline, a build task error is thrown:

`Package '<package> <version>' is not found in the following primary source(s): <paths>`

with `<package>` containing typically an "Avanade" prefixed package name and version likely the "beta" infix.

## Root Cause
One potential root cause can be that the NuGet feed does not contain the referenced package anymore. This is not a very common error but it can happen when beta packages are being cleaned up. Those package are likely cached locally but on the build agent the cache might be cleared and then the package cannot be found.

## Solution
1. Update your package to an (existing) higher version.
1. Test your package.
1. Plan regular package updates for your project (ideally at a point in time when you anyways have to make a regression, for example when a new Release Wave is being tested).
