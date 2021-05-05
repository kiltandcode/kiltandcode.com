---
title: "Puppeteer Sharp: Crawl the Web using C# and Headless Chrome"
date: 2019-04-30 15:26:01 +00:00
author: steven
layout: post
permalink: /puppeteer-sharp-crawl-the-web-using-csharp-and-headless-chrome/
image: /assets/img/2019/04/puppeteerdevtools.png
icon: puppeteer
tags: csharp puppeteer-sharp
---

Puppeteer Sharp is a port of the popular [Headless Chrome NodeJS API](https://pptr.dev/) built by Google. **Puppeteer Sharp was written in C#** and released in [2017](https://www.nuget.org/packages/PuppeteerSharp/0.0.1) by [Darío Kondratiuk](http://www.hardkoded.com) to offer the same functionality to .NET developers.

[Puppeteer Sharp](https://www.puppeteersharp.com/) enables a .NET developer to **programmatically control, or *‘puppeteer’* the open-source Google [Chromium](https://www.chromium.org/) web browser**. The convenience of the Puppeteer API is the ability to use a headless instance of the browser, not actually displaying the UI for increased performance benefits.

## Why use Puppeteer Sharp?

If you are a .NET developer, installing the Puppeteer Sharp Nuget package into your project can enable you to achieve:

- **Crawling the web** using a headless web browser
- **Automated testing** of a web application using a test framework
- **Retrieve JavaScript rendered HTML**

In the modern web it is **common for a web application to rely on JavaScript to load the UI**. If you were to programmatically load [Bing Maps](https://www.bing.com/maps) without using Puppeteer, you may be disappointed to receive:

{%
    include image.html
    year='2019'
    month='04'
    file='bingmapsempty.png'
    alt='Bing Maps empty'
%}

In addition to retrieving JavaScript rendered HTML, Puppeteer Sharp is also capable of navigating the website by **injecting HTML;** **interacting with UI elements;** **taking screenshots** or **creating PDFs**, and has many more features currently included in the popular Google NodeJS API.

## Getting Started

To use Puppeteer Sharp in a new or existing .NET project. install the latest version of the [Nuget package](https://www.nuget.org/packages/PuppeteerSharp) *‘PuppeteerSharp’*.

{%
    include image.html
    year='2019'
    month='04'
    file='puppeteersharpnuget.png'
    alt='PuppeteerSharp on NuGet'
%}

The first line of code that is necessary to *‘puppeteer’* a web browser is to download a revision of Chromium to the local machine. This is the browser that Puppeteer Sharp will use to interact with a website.

Fortunately, we can **use C# to download either the default revision, or a revision the developer specifies**. The revision will only download if it does not already exist on the local machine.

```csharp
// Download the Chromium revision if it does not already exist
await new BrowserFetcher().DownloadAsync(BrowserFetcher.DefaultRevision);
```

If the download is successful, you will see the version of the browser necessary to run on your operating system in your project directory:

{%
    include image.html
    year='2019'
    month='04'
    file='chrome-exe.png'
    alt='Chrome EXE'
%}

## Load a Webpage

Now that you have a browser downloaded to your local machine, you can begin to load a webpage and retrieve the JavaScript rendered HTML.

First, we will programmatically **initiate an instance of the headless web browser**, load a new tab and go to *‘https://www.bing.com/maps’*:

```csharp
// Create an instance of the browser and configure launch options
Browser browser = await Puppeteer.LaunchAsync(new LaunchOptions
{
   Headless = true
});

// Create a new page and go to Bing Maps
Page page = await browser.NewPageAsync();
await page.GoToAsync("https://www.bing.com/maps");
```

{%
    include image.html
    year='2019'
    month='04'
    file='bingmaps1.png'
    alt='Bing Maps (1)'
%}

With the webpage successfully loaded in the headless browser, let’s interact with the webpage by searching for a local tourist attraction:

```csharp
// Search for a local tourist attraction on Bing Maps
await page.WaitForSelectorAsync(".searchbox input");
await page.FocusAsync(".searchbox input");
await page.Keyboard.TypeAsync("CN Tower, Toronto, Ontario, Canada");
await page.ClickAsync(".searchIcon");
await page.WaitForNavigationAsync();
```

{%
    include image.html
    year='2019'
    month='04'
    file='bingmaps2.png'
    alt='Bing Maps (2)'
%}

We’re able to use Puppeteer Sharp to interact with the JavaScript rendered HTML of Bing Maps and search for *‘CN Tower, Toronto, Ontario, Canada’*!

If you would like to store the HTML to parse elements such as the address or description, you can easily store the HTML in a variable:

```csharp
// Store the HTML of the current page
string content = await page.GetContentAsync();
```

Once you are finished, **close the browser to free up resources**:

```csharp
// Close the browser
await browser.CloseAsync();
```

## Screenshots and PDF Documents

One of the benefits of Puppeteer Sharp is the ability to generate screenshots and PDF documents of the current page. This can be particularly **useful for debugging purposes;** **automated testing** or to **capture a webpage at a specific resolution**.

If you would like to a take a screenshot of the current page:

```csharp
await page.ScreenshotAsync("C:\\Files\\screenshot.png");
```

{%
    include image.html
    year='2019'
    month='04'
    file='puppeteerscreenshots.png'
    alt='Puppeteer screenshots'
%}

Alternatively, to generate a PDF document of the current page:

```csharp
await page.PdfAsync("C:\\Files\\document.pdf");
```

## Change the View Port

If you require to test a webpage at a specific display size, such as to **view how the page would appear on a mobile handset**, you can use Puppeteer Sharp to change the size of the view port of the current page:

```csharp
// Change the size of the view port to simulate the iPhone X
await page.SetViewportAsync(new ViewPortOptions
{
    Width = 1125,
    Height = 2436
});
```

{%
    include image.html
    year='2019'
    month='04'
    file='bingmapsiphonexviewport.png'
    alt='Bing Maps iPhone X viewport'
%}

## Trace Logs

Whilst the functionality discussed thus far is useful to monitor and detect issues related to the user interface of a webpage, a .NET developer may also use Puppeteer Sharp to closely examine any network performance issues.

To accomplish this we can programmatically start and stop a trace log:

```csharp
await page.Tracing.StartAsync(new TracingOptions { Path = "C:\\Files\\trace.json" });

...

await page.Tracing.StopAsync();
```

{%
    include image.html
    year='2019'
    month='04'
    file='bingmapstracelogs.png'
    alt='Bing Maps trace log'
%}

If a trace log is not capturing the amount of detail you require in your debugging session, you can programmatically enable Chrome DevTools to yield further insight:

```csharp
Browser browser = await Puppeteer.LaunchAsync(new LaunchOptions
{
   Devtools = true
});
```

If you enable Chrome DevTools in Puppeteer Sharp, **the headless configuration will automatically be disabled** and you will be able to view the browser whilst DevTools displays the options to **view the JavaScript rendered code of your web application**, **view network activity** among other features.

{%
    include image.html
    year='2019'
    month='04'
    file='puppeteerdevtools.png'
    alt='Puppeteer dev tools'
%}

## Connect to a Remote Browser

One last feature of Puppeteer Sharp that I would like to mention is the ability to connect to a remote browser. **This may be useful if you are using a serverless environment where installing a browser is not an option**, such as the scalable *‘Azure Functions’*.

One such service that compliments this feature is [browserless.io](https://www.browserless.io/):

{%
    include image.html
    year='2019'
    month='04'
    file='browserless.png'
    alt='Browserless'
%}

```csharp
var connectOptions = new ConnectOptions()
{
   BrowserWSEndpoint = "$wss://chrome.browserless.io/"
};

using (var browser = await Puppeteer.ConnectAsync(connectOptions))
{
   ...
}
```

## Contribute to Puppeteer Sharp

If you would like to use this excellent API, be sure to visit [puppeteersharp.com](https://www.puppeteersharp.com/). However, If you would like to contribute to this project, you can find the GitHub profile at [github.com/kblok/puppeteer-sharp](https://github.com/kblok/puppeteer-sharp).
