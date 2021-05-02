---
title: "Using Verify, Setup and Callback in the Moq Mocking Framework"
date: 2021-03-11 14:00:00 +00:00
author: "Steven McLintock"
layout: post
image: /assets/img/2021/03/moq.png
icon: moq
tags: csharp unit-testing moq
---

Back in 2019 I wrote an article on [Best Practices for Writing Unit Tests in C# for Bulletproof Code](https://www.kiltandcode.com/2019/06/16/best-practices-for-writing-unit-tests-in-csharp-for-bulletproof-code/). This has become one of my 
more popular articles, and despite it approaching 2 years old, the best practices mentioned are still 
relevant today. I touched upon the popular mocking framework [Moq](https://github.com/moq/moq4), but I 
didn't really go into much detail. This article is intended to explain 
the **Verify**, **Setup** and **Callback** features of Moq with examples of how to use them.

## Demo Console App

To have a piece of code to unit test, and also a dependency to mock I created a demo console app that 
will reverse a word and display it to the user. It uses the class **WordUtils** that has a dependency of 
**UtilLogger** *(the dependency we'll be mocking)* that's used to log a message to the 
console if this has been enabled in appsettings.json. You will also notice the word that's being reversed 
is stored in a simple cache *(a dictionary object)*; this will be used to demonstrate the callback 
functionality.

{%
    include image.html
    year='2021'
    month='03'
    file='moq-demo-console-app.png'
    alt='Demo Console App'
    caption='Demo Console App'
%}

### WordUtils.cs

```csharp
public class WordUtils : IWordUtils
{
    private readonly IUtilLogger _log;

    public const string REVERSE_ERROR_ENTER_A_WORD = 
        "Please enter a word to reverse.";

    public WordUtils(IUtilLogger log)
    {
        _log = log;
    }

    public string Reverse(string word)
    {
        if (string.IsNullOrEmpty(word))
            throw new ArgumentException(REVERSE_ERROR_ENTER_A_WORD);

        char[] charArray = word.ToCharArray();
        Array.Reverse(charArray);
        string reverseWord = new string(charArray);

        if (_log.IsLogEnabled())
            _log.LogInformation(
                $"The word \"{word}\" was reversed as \"{reverseWord}\"");

        // Add the word and the reversed word to the 
        // in-memory cache (a dictionary object)
        _log.AddToWordCache(word, reverseWord);

        return reverseWord;
    }
}
```

### UtilLogger.cs

```csharp
public class UtilLogger<T> : Logger<T>, IUtilLogger
{
    private readonly IOptions<LogOptions> _logOptions;

    public UtilLogger(
        IOptions<LogOptions> logOptions,
        ILoggerFactory logFactory)
        : base(logFactory)
    {
        _logOptions = logOptions;
    }

    private Dictionary<string, string> WordCache { get; set; } = 
        new Dictionary<string, string>();

    public bool IsLogEnabled() 
        => _logOptions.Value.IsLogEnabled;

    public void AddToWordCache(string word, string reverseWord) 
        => WordCache.Add(word, reverseWord);
}
```

## Initialize the Mock and Class Under Test

Before we jump into the verify, setup and callback features of Moq, we'll use 
the *[TestInitialize]* attribute in the MSTest framework to run a method before 
each test is executed. This method will define a mock of the IUtilLogger interface 
and initialize the class under test *(WordUtils)*.

```csharp
private IUtilLogger _log;
private IWordUtils _wordUtils;

[TestInitialize]
public void Initialize()
{
    // Initialize a mock of the logger dependency
    _log = Mock.Of<IUtilLogger>();

    // Initialize the WordUtils class with it's dependency
    _wordUtils = new WordUtils(_log);
}
```

## Using Verify

Now that we've got our mock created, let's write our first unit test to use the 
**verification** feature of Moq. Once we've executed the method that we're 
testing *(Reverse)*, we want to determine if we're actually checking if the 
log is enabled. To do this we'll ***"verify"*** if the *'IsLogEnabled'* method 
on the mock was executed as part of the test. We can also verify how many times 
the method was executed:

```csharp
[TestMethod]
public void Reverse_ShouldInvokeOnce_IsLogEnabled()
{
    _wordUtils.Reverse("mountain");

    Mock.Get(_log).Verify(x => x.IsLogEnabled(), Times.Once);
}
```

## Using Setup

We can also verify if the log was actually logged using the *'Log'* method, 
but to do that we'll need to use the ***"setup"*** feature. If we simply try 
and verify if the log was logged the test will fail. This is because the method 
*'IsLogEnabled'* will return false by default. It will only return true if the 
setting is enabled in appsettings.json, a file that's missing from our unit 
test project. Instead, let's setup the method *'IsLogEnabled'* to simulate 
a response of true:

```csharp
[TestMethod]
public void Reverse_ShouldInvokeOnce_LogInformationMethod()
{
    Mock.Get(_log)
        .Setup(x => x.IsLogEnabled())
        .Returns(true);

    _wordUtils.Reverse("mountain");

    Mock.Get(_log).Verify(x => x.Log(
        LogLevel.Information, 
        It.IsAny<EventId>(), 
        It.IsAny<FormattedLogValues>(), 
        It.IsAny<Exception>(), 
        It.IsAny<Func<object, Exception, string>>()), 
        Times.Once);
}
```

## Using Callback

Imagine if you had a method in your application that inserted a new row into a 
table in a database, and this method was dependant on that new row being 
inserted for it to carry on its work. If your database was a mock in a unit 
test, how would you write a test to simulate that the row was inserted? This 
would be an opportunity to use the ***"callback"*** feature of Moq.

In this example I'm instructing the method *'AddToWordCache'* to insert 
it's two parameters *(word and reverseWord)* into a dictionary object that 
I'm setting up in my unit test. When the method *'AddToWordCache'* is 
executed as part of the test, I can rely on those two parameters being 
inserted into the cache so I can continue with the rest of the unit test.

```csharp
[TestMethod]
public void Reverse_ShouldInvokeOnce_AddToWordCache()
{
    string word = "mountain";
    string reverseWord = "niatnuom";

    Dictionary<string, string> wordCache = new Dictionary<string, string>();

    Mock.Get(_log)
        .Setup(x => x.AddToWordCache(word, reverseWord))
        .Callback<string, string>((w, r) => wordCache.Add(w, r));

    _wordUtils.Reverse(word);

    wordCache.Count.Should().BeGreaterThan(0);

    Mock.Get(_log).Verify(x => x.AddToWordCache(
        It.IsAny<string>(), It.IsAny<string>()), Times.Once);
}
```

## GitHub Repository

The code in this article is available at 
[github.com/kiltandcode/moq-demo-project](https://github.com/kiltandcode/moq-demo-project).