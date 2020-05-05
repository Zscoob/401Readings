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
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
## Universal Windows Apps (UWP) – Windows 10 applications

### Using Devices Runner
  - File -> New Project and create a Blank UWP project
  - Add the xunit.runner.devices package. If you want unit tests in this project, also add xunit
  - Replace App.xaml and App.xaml.cs with the following (using your namespace in x:Class)
    - App.xaml 
___________________________________________________________________________________________
<ui:RunnerApplication
    x:Class="UwpTestRunner.App"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:ui="using:Xunit.Runners.UI"
    RequestedTheme="Light">
</ui:RunnerApplication>      
___________________________________________________________________________________________
  - App.xaml.cs
___________________________________________________________________________________________
using System.Reflection;
using Xunit.Runners.UI;
namespace UwpTestRunner
{
    sealed partial class App : RunnerApplication
    {
        protected override void OnInitializeRunner()
        {
            // tests can be inside the main assembly
            AddTestAssembly(GetType().GetTypeInfo().Assembly);
            // otherwise you need to ensure that the test assemblies will
            // become part of the app bundle
            // AddTestAssembly(typeof(PortableTests).GetTypeInfo().Assembly);
        }
    }
}
___________________________________________________________________________________________
  - Delete MainPage.xaml and MainPage.xaml.cs
  - Add your unit tests
  - (optional) If your tests are in other assemblies, add a project reference and modify the App.xaml.cs to include the assembly containing your tests
  - (optional) Add a xunit.runner.json file as Content to specify runner configuration

-------------------------------------------------------------------------------------------
### Using Visual Studio Test Explorer

#### Create a unit test project
* Visual Studop 2015 or later has the Universal Windows Application tooling
  
  - Open Visual Studio, and choose File > New > Project
    - Unite Test App (Universal Windows)
      - This creates a unit test project with a reference to MSTest which 
      - remove by deleting the reference to MSTestFramework.Universal
#### Add a reference to xUnit.net
  - within Solution Explorer
  - find/open project.json
  - add dependencies on packages:
    - xunit
    - xunit.rummer.visualstudio
  - example below
___________________________________________________________________________________________
{
  "dependencies": {
    "Microsoft.NETCore.UniversalWindowsPlatform": "5.0.0",
    "xunit": "2.1.0",
    "xunit.runner.visualstudio": "2.1.0"
  },
  "frameworks": {
    "uap10.0": { }
  },
  "runtimes": {
    "win10-arm": { },
    "win10-arm-aot": { },
    "win10-x86": { },
    "win10-x86-aot": { },
    "win10-x64": { },
    "win10-x64-aot": { }
  }
}
___________________________________________________________________________________________
#### Write your first tests
  - In the created project UnitTest.cs was auto-created and opened
  - replace contents with the following
___________________________________________________________________________________________
using Xunit;

namespace MyFirstUWPTests
{
    public class UnitTest
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
  - build the solution to make sure the code compiles

#### Running tests with Visual Studio
  - The xunit.runner.visualstudio package that you added earlier allows you to run the tests inside of Visual Studio
  - to see tests
    - ensure code has been compiled
    - show Test Explorer (Test > Windows > Test Explorer)
    - choose runtime environment 
      - Local Machine (Only Win10)
      - Mobile Emulator (VMs that run WinMobile10)
    - Once selected click "Run All"
      - unit test app launch on local machine
      - Windows Mobile 10 UI pop up
    - Once tests have finished results will populate in the Test Explorer UI

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

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

## Xamarin – Android and iOS applications
### Getting Started with xUnit.net
#### Xamarin Android and iOS with the Devices Runner
  - If you want to run your xUnit tests on a supported device, then the device runners are the way to do it. Running unit tests on a device is the surest way to ensure correct behavior, after things like Ahead of Time (AOT) compilation, .NET Native or other device-specific transformations happen. There’s simply no substitute for running “on the metal.”
#### The general pattern
  - xUnit for Devices projects all follow a similar approach: create a new blank application to host the runner, add xunit.runner.devices and tell it which assemblies contain your unit tests. The unit tests may live directly within the application you created or in other referenced libraries, including PCLs.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
## Misc Topics
### What NuGet packages should I use?
#### Packages for writing tests
  - xunit
    - This is the package that will most typically be used by unit test authors.
    - It brings in references to xunit.core (which contains the unit testing framework)
    - xunit.analyzers (which contains source code analyzers)
    - and xunit.assert (which contains the class you use to write assertions).

  - xunit.assert
    - This package contains the xUnit.net assertion library (i.e., the Assert class).
    - This is a separate NuGet package, because some developers wish to use the xUnit.net framework and test runners, but with a different assertion library.
    - If you want to extend the Assert class, you should consider using the xunit.assert.source package instead.

  - xunit.analyzers
    - This package contains the xUnit.net source code analyzers.
    - This library provides code analysis and code fixers for common issues that are encountered both by test authors and extensibility authors. 
    - Based on the .NET Compiler Platform ("Roslyn") analyzers which can provide real-time source code analysis inside IDEs (including Visual Studio and Visual Studio Code), as well as compile-time source code analysis.

  - xunit.core
    - This package contains the core types for the test framework (f.e., FactAttribute).
    - Referencing this package includes xunit.core.dll and also copies (but does not reference) the appropriate execution libraries (xunit.execution.*.dll) which are required to support discovering and executing tests.
    - If you wish to use xunit.core.dll for extensibility purposes (for example, to write your own reusable theory data providers), you should reference xunit.extensibility.core instead.

#### Packages for running tests
  - xunit.runner.console
    - This package contains the console test runner. 
    - This runner is capable of running .NET Framework projects from xUnit.net v1 and v2.
    - To run .NET Core projects from the command line (with dotnet test), please reference xunit.runner.visualstudio instead.

  - xunit.runner.devices
    - This package contains the devices runner. 
    - This runner can execute tests on physical and virtualized devices (iOS and Android via Xamarin, and UWP) from xUnit.net v2.
    - To run UWP projects from within Visual Studio, please reference xunit.runner.visualstudio instead.

  - xunit.runner.msbuild
    - This package contains the MSBuild test runner.
    - This runner is capable of running .NET Framework projects from xUnit.net v1 and v2. 
    - When added, it automatically references the xunit MSBuild target, which can be used in your project file.

  - xunit.runner.visualstudio
    - This package contains the VSTest runner.
    - This runner is capable of running
      - .NET Framework projects from xUnit.net v1 and v2, 
      - .NET Core and UWP projects projects from xUnit.net v2.
    - The VSTest framework is used by several 3rd party runner UIs, including:
      - Visual Studio (via Test Explorer)
      - Visual Studio Code (via .NET Test Explorer)
      - dotnet test and dotnet vstest
      - vstest.console.exe

#### Packages for developers extending xUnit.net
  - xunit.abstractions
    - This package is an internal dependency of the test framework
      - the platform specific execution
        - DLLs (xunit.execution.*.dll)
        - and the runner utility libraries.
      - It defines a stable set of interfaces that these components use to communicate with each other.
      - As long as the interfaces do not change, each component can be modified without requiring any changes or rebuilds of the other components. 
        -This allows for a version independent runner API.
    - You usually won't need to directly reference this package.

  - xunit.assert.source
    - This package contains the xUnit.net assertion library (i.e., the Assert class) in source form.
      - The Assert class is a partial class
        - allows developers to write additional methods available directly from Assert.
    - When you have multiple unit test libraries in your project:
      - it is common practice to import this package into a "test utility" library where you write all your custom assertions
      - and then reference that "test utility" library from your unit test projects.

  - xunit.extensibility.core
    - This package contains xunit.core.dll. 
    - It is intended to be used by developers who wish to reference this DLL for extensibility purposes
      - such as writing your own theory data provider.
    - This package is used by xunit.core.
      - It differs in that it does not include or copy the platform specific execution DLLs (xunit.execution.*.dll).

  - xunit.extensibility.execution
    - This package contains the execution libraries for all Supports (xunit.execution.*.dll).
    - It is intended to be used by developers who wish to reference this DLL for extensibility purposes
      - such as creating custom test discovery and execution.
    -It differs from the xunit.core package in that it adds a reference to the platform specific execution DLL, rather than simply copying to the project's output folder.

  - xunit.runner.utility
    - This package contains the runner utility libraries for all Supports (xunit.runner.utility.*.dll). 
    - It is intended to be used by developers who are writing their own test runners
    - provides a version independent API for discovering and executing tests.
    - The libraries contained here are both backward and forward compatible for all v1 and v2 xUnit.net tests.

#### Retired Packages
  - dotnet-xunit
    - This package was retired as of 2.4 Beta 2.
    - .NET Core users should use xunit.runner.visualstudio and the dotnet test command line.

  - xunit.execution
    - This package was renamed to xunit.extensibility.execution as of 2.0 RC1.

  - xunit.extensions
    - This package was part of xUnit.net v1.x
    - no longer shipped as part of xUnit.net.
    - Some of the features of this package were integrated into the xUnit.net core platform, and others were moved to the xUnit.net samples project.
    - For more information, see upgrading xunit.extensions.

  - xunit.runner.aspnet
    - This package was retired as of 2.1 Beta 2.

  - xunit.runner.dnx
    - This package was retired as of 2.3.

  - xunit.runner.visualstudio.win8
    - This functionality was rolled into xunit.runner.visualstudio.

  - xunit.runner.visualstudio.wpa81
    - This functionality was rolled into xunit.runner.visualstudio.

  - xunit.runners
    - This package was part of xUnit.net v1.x-
    - is no longer shipped as part of xUnit.net
    - It has been replaced with two new packages:
      - xunit.runner.console 
      - xunit.runner.msbuild
      
-------------------------------------------------------------------------------------------
### Multi-targeting on non-Windows Oses
