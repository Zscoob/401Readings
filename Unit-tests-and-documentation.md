# Unit Tests and Documentation
  401 class 2 readings
## Unit Testing
### Unit testing is not
  - Code that isn't stuffed into a database or that reads a file.
  - do not deal with environement or external systems
  - code that can fail when run on a machine without "proper setup"
  - tests that throw multiple requests are smoke tests not unit tests
  - do not generate random data
  - not something QA normally executes 
  - do not exercise multiple components of your system and how they act, end-to-end tests (command line to output from a console app) are not unit tests

### What Unit testing is
  - Unit tests isolate and exercise specific units of your code. 
  - in C# a unit test tests a method
  - tests specifics about methods in isolation 

### Unit Testing Tutorial

Calculator.cs

public class Calculator
{
    public int Add(int x, int y)
    {
        return x + y;
    }
}
_____________________________________________

CalculatorTester

class Program
{
    static void Main(string[] args)
    {
        var calculator = new Calculator();

        int result = calculator.Add(5, 6);

        if (result != 11)
            throw new InvalidOperationException();
    }
}

 - Chhoose the “Unit Test Project Template” after selecting “Test.”
 [TestClass]
public class UnitTest1
{
    [TestMethod]
    public void TestMethod1()
    {
        var calculator = new Calculator();

        int result = calculator.Add(4, 3);

        Assert.AreEqual(7, result);
    }
}

### Anatomy of a unit test

[TestClass]
public class CalculatorTests
{
    [TestMethod]
    public void Adding_4_And_3_Should_Return_7()
    {
        var calculator = new Calculator();

        int result = calculator.Add(4, 3);

        Assert.AreEqual(7, result);
    }
}

  - class called “CalculatorTests,” indicating that it will contain tests for the calculator class.
  - It gets an attribute called “TestClass” to tell Visual Studio’s default test runner and framework, MSTest, that this class contains unit tests.
  - Then, we have a method now called “Adding_4_And_3_Should_Return_7.”
  -  static method Assert.AreEqual.  Microsoft supplies a UnitTesting namespace with this Assert class in it.  
  - Assertion passes and fails determine whether the test passes or fails as seen in the test runner. 

### Unit Testing Best Practices
  - Arrange, Act, Assert
      - Lean heavily on the scientific method to understand the real idea here.  Consider your test as a hypothesis and your test run as an experiment. 
      - first, we arrange everything we need to run the experiment. 
      - Second, the “act” represents the star of the unit testing show.  All of the arranging leads up to it, and everything afterward amounts to retrospection.
      - Third, we assert concept in the unit test represents a general category of action that you cannot omit and have a unit test.  It asserts the hypothesis itself.  Asserting something represents the essence of testing.

  - One Assert per test method
      - should shoot for one assert per test method.  Each test forms a hypothesis and asserts it.  
     - unit test suite should have a test to assert ratio near 1.

  - Avoid test interdependence
      - Each test should handle its own setup and tear down. 
      - don't count on the test suite or the class that you’re testing to maintain state in between tests.
      - If you have two tests, for instance, the test runner may happen to execute them in the same order each time.

  - Keep it short, sweet, and visible
      - esist the impulse to abstract test setup (the “arrange”) to other classes, and especially resist the impulse to abstract it into a base class. 
      - test where all setup logic reveals itself to you at a glance. 

  - Recognize test setup pain as a smell
      - If you find that the “arrange” part of your unit test becomes cumbersome, stop what you’re doing.  One of the most undercover powerful things about unit tests is that they provide excellent feedback on the design of your code — specifically its modularity.  If you find yourself laboring heavily to get a class and method setup so that you can test it, you have a design problem.
      - When you create setup heavy tests, you create brittle tests. AVOID

  - add them to the build
      - If your team has a continuous integration build, add your new unit test suite’s execution to the build.  If any tests fail, then the build fails.  

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
*******************************************************************************************
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
# Documentation

## .NET Core (cross-platform)
  - download if you do not have it URL(https://dotnet.microsoft.com/download)
  - check .NET version = dotnet --version
### create unit test project
  - $ mkdir MyFirstUnitTests
  - $ cd MyFirstUnitTests
  - $ dotnet new classlib

  - open .csproj file, Should look as below:
___________________________________________________________________________________________
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.NET.Test.Sdk" Version="15.9.0" />
    <PackageReference Include="xunit" Version="2.4.1" /> 
          // be sure it's the latest Version on your machine
    <PackageReference Include="xunit.runner.visualstudio" Version="2.4.1" />
          // added a new package references to xunit and xunit.runner.visualstudio.
          // The former is the unit testing framework, and the latter is the library 
          // that enables support for both command line and Visual Studio execution.
  </ItemGroup>

</Project>

___________________________________________________________________________________________
  -verify it works using = $ dotnet test

### Write your first tests
  - Class1.cs is the default new file that populates.
  - test example below:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
using Xunit;

namespace MyFirstUnitTests
{
    public class Class1
    {
        [Fact]
        public void PassingTest()
        {
            Assert.Equal(4, Add(2, 2));
        }

        [Fact]
        public void FailingTest()
        {
            Assert.Equal(5, Add(2, 2));
        }

        int Add(int x, int y)
        {
            return x + y;
        }
    }
}

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  - $ dotnet test //to run test

### Write first theory
   - xUnit.net includes support for two different major types of unit tests: facts and theories.
    - Facts are tests which are always true. They test invariant conditions.
    - Theories are tests which are only true for a particular set of data.
  
   - below is an example of a failed theory:
___________________________________________________________________________________________
[Theory]
[InlineData(3)]
[InlineData(5)]
[InlineData(6)]
public void MyFirstTheory(int value)
{
    Assert.True(IsOdd(value));
}

bool IsOdd(int value)
{
    return value % 2 == 1;
}
___________________________________________________________________________________________

### Running tests againat multiple targets
  - .NET Core 1.0 or later, Desktop .NET 4.5.2 or later
  - Open the .csproj file and change this:
  <PropertyGroup>
    <TargetFramework>netcoreapp2.1</TargetFramework>
  </PropertyGroup>
  
  To this:
  <PropertyGroup>
    <TargetFrameworks>net452;netcoreapp2.1</TargetFrameworks>
  </PropertyGroup>

  - TargetFramework is now TargetFrameworks
  - net452; was added before the netcoreapp

  - Since we changed the .csproj file we need to run " $ dotnet restore " before running the unit tests.
  - run $ dotnet test 

### Running tests with Visual Studio
  - Upgrade to Community edition if you havent already
  - Make sure Test Explorer is visible (go to Test > Windows > Test Explorer).
  - Click the Run All link in the Test Explorer window
  - You can click on a failed test to see the failure message, and the stack trace.
  - You can click on the stack trace lines to take you directly to the failing line of code.
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

## .NET Framework
### Visual Studio (Windows)
#### Create a class library project
  - .NET 4.5.2 (or later). Open Visual Studio
   - choose File > New > Project
  - Add a reference to xUnit.net
    - right click the new project
    - choose Manage NuGet Packages
      - Along the top edge, make sure Browse is selected.
      - On the upper right side, ensure nuget.org is selected under Package source
      - In the search box, type xunit
      - Select the xUnit.net package, and click Install
  - (xunit) is what's called a meta-package
    - brings packages that include the core unit testing framework and the assertion framework 
  - If you open packages.config, you'll see all the packages that get referenced

  -Package example below:
<?xml version="1.0" encoding="utf-8"?>
<packages>
  <package id="xunit" version="2.2.0" targetFramework="net452" />
  <package id="xunit.abstractions" version="2.0.1" targetFramework="net452" />
  <package id="xunit.assert" version="2.2.0" targetFramework="net452" />
  <package id="xunit.core" version="2.2.0" targetFramework="net452" />
  <package id="xunit.extensibility.core" version="2.2.0" targetFramework="net452" />
  <package id="xunit.extensibility.execution" version="2.2.0" targetFramework="net452" />
</packages>
//When creating a new project with Visual Studio 2017,
//you may not end up with a packages.config file.
//newer versions of Visual Studio place the NuGet package references
//directly into your .csproj file as <PackageReference>

#### Write your first tests
  - Class1.cs is the default new file
    - below is an example test:
___________________________________________________________________________________________
using Xunit;

namespace MyFirstUnitTests
{
    public class Class1
    {
        [Fact]
        public void PassingTest()
        {
            Assert.Equal(4, Add(2, 2));
        }

        [Fact]
        public void FailingTest()
        {
            Assert.Equal(5, Add(2, 2));
        }

        int Add(int x, int y)
        {
            return x + y;
        }
    }
}    
___________________________________________________________________________________________
 - Now that you've written the first test, we need a way to run it. Let's install the NuGet package with the console runner.
#### Add a reference to xUnit.net console runner
  - Right click on the project in Solution Explorer and choose Manage NuGet Packages
  - search for, and install a package named " xunit.runner.console "
    - this package is a solution-level package
#### Run tests with the xUnit.net console runner
  - Open a command prompt or PowerShell command window. In the window, navigate to the root folder of your solution.
  - to run the console runner your command should be simmilar to:
    - packages\xunit.runner.console.2.2.0\tools\xunit.console MyFirstUnitTests\bin\Debug\MyFirstUnitTests.dll
#### Write your first theory
  - xUnit.net includes support for two different major types of unit tests: facts and theories.
    - Facts are tests which are always true. They test invariant conditions.
    - Theories are tests which are only true for a particular set of data.
  - below is an example of a failed theory:
___________________________________________________________________________________________
[Theory]
[InlineData(3)]
[InlineData(5)]
[InlineData(6)]
public void MyFirstTheory(int value)
{
    Assert.True(IsOdd(value));
}

bool IsOdd(int value)
{
    return value % 2 == 1;
}
___________________________________________________________________________________________
#### Running tests with Visual Studio
*  If you've previously installed the xUnit.net Visual Studio Runner VSIX (Extension), you must uninstall it first. The Visual Studio runner is only distributed via NuGet now. To remove it, to go Tools > Extensions and Updates. Scroll to the bottom of the list, and if xUnit.net is installed, uninstall it. This will force you to restart Visual Studio.
* If you're having problems discovering or running tests, you may be a victim of a corrupted runner cache inside Visual Studio. To clear this cache, shut down all instances of Visual Studio, then delete the folder %TEMP%\VisualStudioTestExplorerExtensions. Also make sure your solution is only linked against a single version of the Visual Studio runner NuGet package (xunit.runner.visualstudio).

  - run your xUnit.net tests within Visual Studio Community Edition built-in test runner named Test Explorer
    - Right click on the project in Solution Explorer and choose Manage NuGet Packages
    - Search for, and install a package named xunit.runner.visualstudio
  - go to Test > Windows > Test Explorer to ensure tests explorer is visible.
  - every time you build your project the runner will discover unit tests
    - click " Run All" in the Text Explorer window to run your tests
  - You can click on a failed test to see the failure message, and the stack trace.
  - You can click on the stack trace lines to take you directly to the failing line of code.
#### Console runner return codes
  - Return code ==	Meaning
  -     0       ==  The tests ran successfully.
  -     1       ==	One or more of the tests failed.
  -     2       ==  The help page was shown, either because it was requested, or because 
                    the user did not provide any command line arguments.
  -     3       ==  There was a problem with one of the command line options passed to the 
                    runner.
  -     4       ==  There was a problem loading one or more of the test assemblies (for 
                    example, if a 64-bit only assembly is run with the 32-bit test runner).
  - 0xC000013A (-1073741510)  ==  The user canceled the test execution by pressing Ctrl+C.
-------------------------------------------------------------------------------------------
### JetBrains Rider (cross-platform)
#### Create a test project
  - Example below is a project called Sandbox with a class Calculator
___________________________________________________________________________________________namespace Sandbox
{
   public class Calculator
   {
       public static int Add(int x, int y) => x + y;
       public static int Subtract(int x, int y) => x - y;
   }
}
___________________________________________________________________________________________
  - to test the calculator:
    - creating a project for our xUnit.net tests
      - In the Solution Explorer
      - right-click the solution
      - choose Add > New Project...
    - Choose the Unit Test Project template targeting .NET 4.5.2 or later
      - select xUnit as the project type
      - provide some telling name for it, e.g. Tests
    - Click Create
      - the new test project with all necessary configurations and references will be added to our solution.

#### Write your first tests
  - default new file is names Tests.cs
    - example code below:
___________________________________________________________________________________________
using Sandbox;
using Xunit;

namespace Tests
{
   public class Tests
   {
       [Fact]
       public void PassingTest()
       {
           Assert.Equal(4, Calculator.Add(2, 2));
       }

       [Fact]
       public void FailingTest()
       {
           Assert.Equal(5, Calculator.Add(2, 2));
       }
   }
}
___________________________________________________________________________________________
* If you copy-pasted the above code to the Tests.cs file, the Calculator usage will be highlighted as unresolved because the Sandbox project is not referenced in our test project. You can press Alt+Enter on the highlighted usage to add the missing project reference.

  - click the unit test icon next to the test class and choose Run All to run all tests in that class
  - Unit test results will populate in a window
    - PassingTest = pass
    - FailingTest = fail

#### Writw your first theory
  - xUnit.net includes support for two different major types of unit tests: facts and theories.
    - Facts are tests which are always true. They test invariant conditions.
    - Theories are tests which are only true for a particular set of data.
  - below is an example of a failed theory:
___________________________________________________________________________________________
[Theory]
[InlineData(2, 2, 4)]
[InlineData(3, 3, 6)]
[InlineData(2, 2, 5)]
public void MyTheory(int x, int y, int sum)
{
   Assert.Equal(sum, Calculator.Add(x, y));
}
___________________________________________________________________________________________
  - to run additional unit tests in the Solution Explorer, right-click on it and choose Run Unit Tests. Or alternatively, you can browse all tests in the solution on the Explorer tab of the Unit Tests window and run tests from there.
