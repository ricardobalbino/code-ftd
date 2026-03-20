# Code Review Checklist

- Git

    - Clean up your Git branches (the branches are typically deleted after a Pull Request is approved but sometimes they are not deleted and you need to delete them)
    
    - Push local changes early and often (keep in mind: every push triggers a guard build. If too many guard builds are blocking other pipelines please [increase the number of parallel jobs in Azure Dev Ops](https://docs.microsoft.com/en-us/azure/devops/pipelines/licensing/concurrent-jobs?view=azure-devops&tabs=ms-hosted))

- Development Environments

    - Remove all unused components regularly or reset the environments via restore and [re-initialization](../../How-Tos/Others/manage-lower-environments.md).

- General Coding (C\# and Javascript)
    - Does the code satisfy the specification?
    - Are there logic issues?
    - Are all the coding standards being followed?
    - Are errors properly handled?
    - Is the exception handling, logging, and tracing sufficient to allow for debugging at runtime?
    - Is the code self-documenting/-explanatory?
    - Have all spelling mistakes in method names/comments been removed?
    - Has all code been properly formatted? (Press Ctrl+K+D (code formatting))
    - Is the code commented, readable and are the comments accurate? (Methods must have comments, preferably referencing and matching the algorithms and logic provided in the design.)
    - Has all commented code been removed?
    - Are there hard coded constant values? If so, should they still be there?
    - Have all defect numbers/CR numbers been removed from the code comments?
    - Are the naming conventions adhered to?
    - Are shared functions leveraged from existing classes (e.g. for string or JSON handling) and is all generic functionality properly refactored?
- C\#
    - Does the code compile?
    - Are all warnings gone?
- Typescript/Javascript/HTML
    - Can the code being bundled (`npm run build-dev`)?
    - Are all lint validation warnings/errors corrected?
- Unit Tests
    - Does the new code have the right unit tests?
    - Do the unit tests cover the business logic accurately?
    - Are the existing unit tests correctly adjusted for modification of existing code?
    - Does each unit test do one thing?
    - Are the unit tests names descriptive?
    - Does each unit test summary comment adhere to the standards? (Summary, Assumption, Condition, ...)
    - Does each unit test adhere to the Arrange / Act / Assert principle?
    - Do the asserts cover as much functional results as possible? (Assertion for example that a function result has some data in it, is not sufficient, it must also be asserted that it is the right data.)
    - Are test cleanups being addressed (`TestCleanup` method)?