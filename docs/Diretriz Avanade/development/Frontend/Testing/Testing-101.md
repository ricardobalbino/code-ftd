# Testing 101

Unit testing is an important topic in software development. It applies to both, backend and frontend. However, often enough developers focus mainly on the backend unit tests and completely ignore writing tests for their frontend code. One of the reasons is that it's often quite annoying and complex to mock all the Javascript/Typescript objects and provide dummies for asyncronous operations. In addition setting up a test runner with code coverage and source transpiling takes quite some time.

The BizApps Core Accelerator tackles this problem and provides a full testing infrastructure for Frontend tests. This includes a fully features test runner setup ([jest](https://jestjs.io/)), including code transpilation with babel, full coverage report and complete integration into the CI/CD Pipeline. Moreover it contains many mock objects and helper classes, which makes it very easy to write unit tests for the frontend.

## AAA Pattern
A test method follows the AAA (Arrange, Act, Assert) pattern which is a common way of structuring unit test methods.

- The Arrange section of a unit test method initializes objects and sets the value of the data that is passed to the method under test.

- The Act section invokes the method under test with the arranged parameters.

- The Assert section verifies that the action of the method under test behaves as expected.

## Mocking

An object under test may have dependencies on other (complex) objects. To isolate the behavior of the object you want to replace the other objects by mocks that simulate the behavior of the real objects. This is useful if the real objects are impractical to incorporate into the unit test.

Services, which contain the actual business logic of the solution, contain references to other classes to fulfill their requirement. These references are hard dependencies which the service depends on. 

Since all dependencies should be abstracted away using interfaces or are provided as constructor parameters - these dependencies are being used to mock/simulate any given behavior.

## Assertion

Assertions verify that a given action behaved or contains a value as expected.

For assertions we're using `Should.js`. Should extends all types with helper methods like `.should.be.true()` or `.should.equal(5)`. For a complete list of possible assertions head on over [here](https://shouldjs.github.io/).

The old way:
```ts
    if (contestant.points !== 1337) {
        throw "Incorrect contestent"
    }
```


Should.js's way:
```ts
    contestant.points.should.equal(1337);
```

As you can see above the example with Should.js is simpler, terse and on point.
