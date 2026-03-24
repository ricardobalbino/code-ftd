# Getting Started

Let's assume we have one service and one interface the service depends on.

```csharp
public class MyService
{
    public MyService(IAuthorize authorizer) {...}

    public double Execute() 
    {
        var isAuthorized = _authorizer.IsUserAuthorized();
        if (!isAuthorized)
            throw new SecurityException("User not authorized to perform this action.");
        
        // Access org service and return value from some field...
        return 100;
    }
}

public interface IAuthorize
{
    bool IsUserAuthorized();
}
```

So the IAuthorize interface is taking care of if the user has the privilege to perform a certain action. The service doesn't know how and what lies behind it but for the service it doesn't matter.

Now let's write a test for MyService:

```csharp
[TestMethod]
public void ReturnsValueWhenUserIsAuthorized()
{
    // Arrange
    var authorizer = Substitute.For<IAuthorize>();
    authorizer.IsUserAuthorized().Returns(true);

    var service = new MyService(authorizer);

    // Act
    var result = service.Execute();

    // Assert
    result.ShouldBe(100);
}
```

> As we've outlined above we're structuring the body of the test method using the AAA pattern. 

First we're creating all objects and mocks. 

```csharp
    var authorizer = Substitute.For<IAuthorize>();
```
This is creating a mock objects based off the IAuthorize interface using NSubstitute. With that we're getting an instance of IAuthorize with which we can add more simulations to it.

```csharp
    authorizer.IsUserAuthorized().Returns(true);
```
Since the authorizer variable contains an instance of the mocked interface we can use its methods like we would use it inside of our production code. All instances created from NSubstitute have extension methods which we can use to simulate the behavior of that method if it gets called. Like it this case we're telling the mocked object to return true if somebody is calling its method ```.IsUserAuthroized()```.

The provided ```.Returns()``` extension method is return value aware which means that depending on the mocked method's return value it takes the same type of parameter.

Second we're executing the actual logic of the service. Typically the act part of the method is a one liner as seen above.

Third we're validating the result if the result has the right value. We're using [Shouldly](https://docs.shouldly.io/) to assert a variables value. Since Shoudly provides a more human readable format it is easier to process (brain processing time) than the traditional ``` Assert.AreEqual(x,y)```  method.
