---
title: "Creating a Web API Using Only the .NET CLI and Visual Studio Code"
date: 2021-04-13 20:15:00:00 +00:00
author: steven
layout: post
image: /assets/img/2021/04/terminal-dotnet.png
icon: vscode
tags: dotnet vscode csharp
---

Have you ever wondered if you could use the **.NET CLI** and **Visual Studio Code** to build a new .NET application, instead of relying on Visual Studio to do all of the heavy lifting?

This tutorial will walk you through creating a new **class library**, **unit test project** and **web API** using only the .NET CLI and Visual Studio Code.

## Prerequisites

* [.NET 5.0 CLI](https://dotnet.microsoft.com/download) *(or later)*
* [Visual Studio Code](https://code.visualstudio.com) for macOS, Windows or Linux
* Install the [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) by [Microsoft](https://www.microsoft.com/) for Visual Studio Code

{%
    include image.html
    year='2021'
    month='04'
    file='csharp-extension.png'
    alt='C# extension'
    caption='C# extension'
%}

Before we get started, let's first check if the .NET CLI is installed correctly by running the following command in the Terminal:

```terminal
dotnet
```

If the .NET CLI is installed on your system *(macOS in this example)* you should see the following:

{%
    include image.html
    year='2021'
    month='04'
    file='terminal-dotnet.png'
    alt='Terminal in macOS running the dotnet command'
    caption='Terminal in macOS running the dotnet command'
%}

## Create a New Solution

Let's begin by creating a new folder for storing the solution and project files. In this example the folder will be named *'DiceRollerApi'*.

Open the new folder in Visual Studio Code and open a new integrated terminal *(View > Terminal)*. This is where we'll be running all of the .NET CLI commands in this tutorial.

Let's create a new solution file using the command:

```terminal
dotnet new sln
```

{%
    include image.html
    year='2021'
    month='04'
    file='new-solution-terminal.png'
    alt='Using the .NET CLI to create a new solution'
    caption='Using the .NET CLI to create a new solution'
%}

The solution file will be automatically named the same as the directory you currently reside in. In this case the solution file is named *'DiceRollerApi.sln'*.

{%
    include image.html
    year='2021'
    month='04'
    file='new-solution-gui.png'
    alt='New solution file'
    caption='New solution file'
%}

If you would like to explore all of the commands available in the .NET CLI, you can check out the Microsoft [dotnet command](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet) documentation.

### Why Use a Solution in Visual Studio Code?

The solution file is traditionally used by Visual Studio to keep track and organize all of the project files in your application. However, the solution file isn't necessarily required in Visual Studio Code, it's optional.

There are a couple of reasons why you might want to use a solution file in Visual Studio Code:

* If you or another developer would like to maintain the application in Visual Studio in addition to Visual Studio Code
* Instead of building each project separately using the .NET CLI, we can use the command ***'dotnet build DiceRollerApi.sln'*** to build the entire solution.

## Create a New Class Library

We'll create a new class library that will use the NuGet package [DiceRoller](https://www.nuget.org/packages/DiceRoller/) to roll one or many dice.

{%
    include image.html
    year='2021'
    month='04'
    file='dice-roller-nuget.png'
    alt='DiceRoller NuGet package'
    caption='DiceRoller NuGet package'
%}

Our class **‘RollDice'** will contain the method **‘Roll'** that accepts two parameters, the number of dice to use in our roll, and how many sides each die will have. Let's create the new class library **'DiceRoller.DiceRoll'** by using the following command:

```terminal
dotnet new classlib -n DiceRoller.DiceRoll
```

{%
    include image.html
    year='2021'
    month='04'
    file='new-classlib-gui.png'
    alt='New class library'
    caption='New class library'
%}

Because we're using the .NET CLI and Visual Studio Code, we'll need to add the new project to our solution file manually, so let's do that using the command:

```terminal
dotnet sln add DiceRoller.DiceRoll/DiceRoller.DiceRoll.csproj
```

{%
    include image.html
    year='2021'
    month='04'
    file='add-classlib-to-solution.png'
    alt='Add class library to solution'
    caption='Add class library to solution'
%}

We'll also use the .NET CLI to add the [DiceRoller](https://www.nuget.org/packages/DiceRoller/) NuGet package to the *'DiceRoller.DiceRoll'* class library:

```terminal
dotnet add DiceRoller.DiceRoll/DiceRoller.DiceRoll.csproj package DiceRoller
```

Let's rename the auto-generated class **'Class1'** to **'RollDice'**. Visual Studio Code doesn't seem to automatically rename the file, so be sure to rename the file to **'RollDice.cs'** as well.

{%
    include image.html
    year='2021'
    month='04'
    file='rename-class1.png'
    alt='Rename the auto-generated class'
    caption='Rename the auto-generated class'
%}

We'll also add in the code to call the **'DiceRoller'** NuGet package and pass in our desired parameters. There's also an interface being used that will come in later when we use dependency injection in the web API.

```csharp
using Dice;

namespace DiceRoller.DiceRoll
{
    public class RollDice : IRollDice
    {
        public decimal Roll(int numberOfDice, int sidesPerDie)
        {
            string diceExpr = $"{numberOfDice}d{sidesPerDie}";
            RollResult rollResult = Roller.Roll(diceExpr);
            return rollResult.Value;
        }
    }
}
```

## Create a New Unit Test Project

The .NET CLI has the ability to add any kind of .NET project, including unit test projects using either the MSTest, NUnit or xUnit testing framework. Let's add a unit test project to test our class library:

```terminal
dotnet new mstest -n DiceRoller.DiceRoll.Tests
```

{%
    include image.html
    year='2021'
    month='04'
    file='new-unit-test-project.png'
    alt='New unit test project'
    caption='New unit test project'
%}

We'll also add the new unit test project to our solution file:

```terminal
dotnet sln add DiceRoller.DiceRoll.Tests/DiceRoller.DiceRoll.Tests.csproj
```

To use the class library in a unit test, we need to add a reference to the class library in our unit test project:

```terminal
dotnet add DiceRoller.DiceRoll.Tests/DiceRoller.DiceRoll.Tests.csproj reference DiceRoller.DiceRoll/DiceRoller.DiceRoll.csproj
```

And of course, let's rename the auto-generated **'UnitTest1'** to **'RollDiceTests'** and write a unit test to ensure the result that's returned from a roll of the dice is what we expect:

```csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;

namespace DiceRoller.DiceRoll.Tests
{
    [TestClass]
    public class RollDiceTests
    {
        private RollDice _classUnderTest;

        [TestInitialize]
        public void Initialize() => _classUnderTest = new RollDice();
        
        [TestMethod]
        public void Roll_IsSuccess()
        {
            int numberOfDice = 3;
            int sidesPerDie = 12;

            decimal roll = _classUnderTest.Roll(
                numberOfDice, sidesPerDie);
            
            Assert.IsTrue(roll >= 3);
            Assert.IsTrue(roll <= 36);
        }
    }
}
```

Using the .NET CLI, let's run all of the unit tests within our new unit test project:

```terminal
dotnet test DiceRoller.DiceRoll.Tests/DiceRoller.DiceRoll.Tests.csproj
```

In the integrated terminal, you should receive information on the results of your unit tests ***(hopefully they pass!)***:

{%
    include image.html
    year='2021'
    month='04'
    file='unit-test-passed.png'
    alt='Unit test result'
    caption='Unit test result'
%}

## Create a New Web API Project

With scaffolding being built into the .NET CLI, we can use one command to create a web API with an auto-generated controller, **'Startup.cs'** file and all of the necessary dependencies added as references.

Let's use the following command to create a new web API with scaffolding:

```terminal
dotnet new webapi -n DiceRoller.WebApi
```

{%
    include image.html
    year='2021'
    month='04'
    file='new-webapi-project.png'
    alt='New web API project'
    caption='New web API project'
%}

Don't forget to add the new web API project to the solution file:

```terminal
dotnet sln add DiceRoller.WebApi/DiceRoller.WebApi.csproj
```

Similar to the unit test project, we'll also add a reference to the class library in our web API project:

```terminal
dotnet add DiceRoller.WebApi/DiceRoller.WebApi.csproj reference DiceRoller.DiceRoll/DiceRoller.DiceRoll.csproj
```

Let's rename the auto-generated controller from **'WeatherForecastController'** to **'RollDiceController'** and add a new endpoint to pass the parameters *'numberOfDice'* and *'sidesPerDie'* to the **'Roll'** method in the class library:

{%
    include image.html
    year='2021'
    month='04'
    file='webapi-rename-controller.png'
    alt='Rename the auto-generated controller'
    caption='Rename the auto-generated controller'
%}

```csharp
using Microsoft.AspNetCore.Mvc;
using Microsoft.Extensions.Logging;
using DiceRoller.DiceRoll;

namespace DiceRoller.WebApi.Controllers
{
    [ApiController]
    [Route("[controller]")]
    public class RollDiceController : ControllerBase
    {
        private readonly ILogger<RollDiceController> _logger;
        private readonly IRollDice _rollDice;
        
        public RollDiceController(
            ILogger<RollDiceController> logger,
            IRollDice rollDice)
        {
            _logger = logger;
            _rollDice = rollDice;
        }

        [HttpGet]
        public decimal Roll(int numberOfDice, int sidesPerDie) =>
            _rollDice.Roll(numberOfDice, sidesPerDie);
    }
}
```

Using the interface in the class library, we'll modify the **'ConfigureServices'** method in the **'Startup.cs'** file to inject the **'RollDice'** class into our web API controller:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    ...

    services.AddSingleton<IRollDice, RollDice>();
}
```

Because we've been adding each project to the solution file, we can now use one command to build all of the projects and check for any errors:

```terminal
dotnet build DiceRollerApi.sln
```

Now let's use the .NET CLI to locally run our web API!

```terminal
dotnet run -p DiceRoller.WebApi/DiceRoller.WebApi.csproj
```

{%
    include image.html
    year='2021'
    month='04'
    file='run-webapi-project.png'
    alt='Run the web API project'
    caption='Run the web API project'
%}

Now that the web API is running locally, we can navigate to **https://localhost:5001/swagger** and use [Swagger](https://swagger.io) to check if our endpoint is working the way we expect:

{%
    include image.html
    year='2021'
    month='04'
    file='webapi-swagger-request.png'
    alt='Make a request to the endpoint using Swagger'
    caption='Make a request to the endpoint using Swagger'
%}

{%
    include image.html
    year='2021'
    month='04'
    file='webapi-swagger-response.png'
    alt='The response from the request to the endpoint'
    caption='The response from the request to the endpoint'
%}

I hope this demonstrates how you can use only the .NET CLI and Visual Studio Code to build a web API!