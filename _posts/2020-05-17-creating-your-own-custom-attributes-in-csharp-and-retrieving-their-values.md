---
title: "Creating Your Own Custom Attributes in C# and Retrieving Their Values"
date: 2020-05-17 14:37:00 +00:00
author: "Steven McLintock"
layout: post
image: /assets/img/2020/05/attribute-usage.png
category: csharp
---

You would think after using a programming language for years that you've seen it all - we all know this simply isn't the case. Custom attributes is a feature of C# that I had seen being used by others, but had never actually used myself. Well, it's time to change that!

I was trying to think of an example of where C# attributes might be used in every day life and then it dawned on me: [unit testing](https://kiltandcode.com/2019/06/16/best-practices-for-writing-unit-tests-in-csharp-for-bulletproof-code/)! If you have ever used a unit test framework such as [MSTest](https://docs.microsoft.com/en-us/dotnet/core/testing/unit-testing-with-mstest), [NUnit](https://nunit.org/) or [xUnit](https://xunit.net/), you will have used attributes in C# to define the classes and methods that you would like the test framework to execute. This is just one example of how you can use attributes to assign metadata to elements of your code.

{%
    include image.html
    year='2020'
    month='05'
    file='attributes-for-unit-tests.png'
    alt='TestClass and TestMethod attributes in C#'
    caption='TestClass and TestMethod attributes in C#'
%}

Instead of consuming attributes already built into .NET, there will occasionally be a requirement to create your own custom attribute. A use case for this may be to indicate areas of your application that require a specific user permission. What if you didn't want every user logged in to your API to be able to access a utility to obtain user credentials, for example. With a custom attribute we're able to indicate the user permission(s) required to access this functionality.

## Create a Custom Attribute

Let's begin by creating a new custom attribute by declaring a new class that inherits from the System.Attribute class:

```csharp
[AttributeUsage(AttributeTargets.Class, AllowMultiple = true)]
public class PermissionRequiredAttribute : Attribute
{
    public PermissionRequiredAttribute(string permission)
    {
        Permission = permission;
    }

    public string Permission { get; }
}
```

Ironically, you'll notice there is the attribute **AttributeUsage** being used on the new custom attribute. This is to inform .NET that the new custom attribute should target a class *(as opposed to a method, property, etc.)* and also allow classes to be assigned multiple instances of the custom attribute.

{%
    include image.html
    year='2020'
    month='05'
    file='attribute-usage.png'
    alt='Creating a new custom attribute in C#'
    caption='Creating a new custom attribute in C#'
%}

Be sure to append **"Attribute"** to the end of the name of your new custom attribute. Although the class in the code snippet is named **PermissionRequiredAttribute**, you will actually refer to it as an attribute by simply using **[PermissionRequired]**. Cool, huh!

Let's apply our new custom attribute to a Credentials class to add metadata that only users with certain permission(s) can access it:

```csharp
[PermissionRequired("administrator")]
[PermissionRequired("manager")]
public class Credentials : ICredentials
{
    public string[] GetCredentials()
    {
        throw new NotImplementedException();
    }
}
```

Because we set the AttributeUsage property **AllowMultiple** to true, we're able to assign multiple custom attributes to the Credential class to define the different user permissions, and we can easily add more in the future if needed.

## Retrieving Values of a Custom Attribute

Let's make use of [reflection](https://docs.microsoft.com/en-us/dotnet/framework/reflection-and-codedom/reflection) in .NET to use the method **GetCustomAttributes** to retrieve a list of PermissionRequiredAttribute objects and assign their *Permission* properties to a collection of strings.

```csharp
IEnumerable<string> permissions = 
    typeof(Credentials).GetCustomAttributes(
        typeof(PermissionRequiredAttribute), true)
            .Cast<PermissionRequiredAttribute>()
            .Select(x => x.Permission);
```

We're now able to iterate through the collection and determine the permissions of the class assigned by the custom attribute!

{%
    include image.html
    year='2020'
    month='05'
    file='custom-attributes-terminal.png'
    alt='Retrieving the values of the custom attribute'
    caption='Retrieving the values of the custom attribute'
%}

## Useful links

* GitHub [repository](https://github.com/kiltandcode/custom-attributes) for the code used in this article
* Official documentation on [Writing Custom Attributes](https://docs.microsoft.com/en-us/dotnet/standard/attributes/writing-custom-attributes)
* Official documentation on [Retrieving Information Stored in Attributes](https://docs.microsoft.com/en-us/dotnet/standard/attributes/retrieving-information-stored-in-attributes)