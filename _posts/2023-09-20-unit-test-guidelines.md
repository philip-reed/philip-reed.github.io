---
title: Unit Test Guidelines
date: 2023-09-19 20:16:14 +0100
categories: [Best Practices]
tags: [unit tests, testing, best practice]     # TAG names should always be lowercase
---

Writing a good unit test is a major factor in helping maintain a project.  Some common mistakes can make debugging a failing test in the future much more difficult than it needs to be. Be kind to your future self and colleagues by considering the following guidelines.

### General Guidance
- Avoid introducing dependencies on infrastructure when writing unit tests. Doing so can make the tests slow, and unreliable, and should be reserved for integration testing.  
Instead, dependencies should be mocked, so that only the Subject Under Test (SUT) is being tested.

- Tests should also avoid using branching logic. If a test contains `if` statements, this could be [code smell](https://en.wikipedia.org/wiki/Code_smell), and it may be more appropriate to have another test or have a [Data Driven Test](#data-driven-tests)


### Naming your tests 

The name of your test should consist of three parts:
- The name of the method being tested.
- The scenario under which it's being tested.
- The expected behavior when the scenario is invoked.

> tip Good test names follow the `When_Given_Then` notation or similar, see alternatives.  
> `When` describes the intent.  
> `Given` describes the context.  
> `Then` describes the expectation
{: .prompt-info }

Naming standards are important because they explicitly express the intent of the test.  
Tests are more than just making sure your code works, they also provide documentation. Just by looking at the suite of unit tests, you should be able to infer the behavior of your code without even looking at the code itself. Additionally, when tests fail, you can see exactly which scenarios do not meet your expectations.

:x: Bad test names:
- `Test_Single`, `Test_Double`
- `Test01`, `Test02`, `Test03`, etc...
- `SomeReallyLongTestNameThatIsNotReadable`

::: warning
Bad test names don't explain the use case being tested
:::

:white_check_mark: Good names:
- `Add_SingleNumber_ReturnsSameNumber`
- `Subtract_DoubleNumber_ReturnsLowerNumber`
- `Multiple_ZeroNumber_ThrowsException`

### Arranging your tests
Arrange, Act, Assert is a common pattern when unit testing. As the name implies, it consists of three main actions:
- Arrange your objects, creating and setting them up as necessary.
- Act on an object.
- Assert that something is as expected.

Why?
- Clearly separates what is being tested from the arrange and assert steps.
- Less chance to intermix assertions with "Act" code.

Example:
```csharp
[TestMethod]
public void Add_EmptyString_ReturnsZero()
{
    // Arrange
    var stringCalculator = new StringCalculator();

    // Act
    var actual = stringCalculator.Add("");

    // Assert
    Assert.Equal(0, actual);
}
```

### Data-Driven Tests 

Avoid duplicate tests where the only change is the input parameters.  


```csharp
[TestClass]
public class MathTests
{
    [TestMethod]
    public void Test_Add1()
    {
        var actual = MathHelper.Add(1, 1);
        var expected = 2;
        Assert.AreEqual(expected, actual);
    }

    [TestMethod]
    public void Test_Add2()
    {
        var actual = MathHelper.Add(21, 4);
        var expected = 25;
        Assert.AreEqual(expected, actual);
    }
}
```

Instead, consider using a data-driven test where the parameters can be passed in.

```csharp
[TestClass]
public class MathTests
{
    [DataTestMethod]
    [DataRow(1, 1, 2)]
    [DataRow(12, 30, 42)]
    [DataRow(14, 1, 15)]
    public void Test_Add(int a, int b, int expected)
    {
        var actual = MathHelper.Add(a, b);
        Assert.AreEqual(expected, actual);
    }
}
```

If your data cannot be set into an attribute parameter (non-constant values or complex objects), you can use the `[DynamicData]` attribute. 
This attribute allows getting the values of the parameters from a method or a property. 
The method or the property must return an `IEnumerable<object[]>`.
Each row corresponds to the values of a test.

```csharp
[TestClass]
public class MathTests
{
    [DataTestMethod]
    [DynamicData(nameof(GetData), DynamicDataSourceType.Method)] //Properties also supported
    public void Test_Add_DynamicData_Method(int a, int b, int expected)
    {
        var actual = MathHelper.Add(a, b);
        Assert.AreEqual(expected, actual);
    }

    public static IEnumerable<object[]> GetData()
    {
        yield return new object[] { 1, 1, 2 };
        yield return new object[] { 12, 30, 42 };
        yield return new object[] { 14, 1, 15 };
    }
}

```

More Info [https://www.meziantou.net/mstest-v2-data-tests.htm](https://www.meziantou.net/mstest-v2-data-tests.htm)
