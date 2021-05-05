---
title: "Scrape the 500 Greatest Albums of All Time using Puppeteer Sharp"
date: 2020-10-14 19:00:00 +00:00
author: steven
layout: post
image: /assets/img/2019/04/puppeteer-logo.png
icon: puppeteer
tags: csharp puppeteer-sharp
---

As a keen [Spotify](https://www.spotify.com/) user that is always in search of new albums and artists to 
listen to, I was particularly interested when I heard that [Rolling Stone](https://www.rollingstone.com/) 
magazine announced a new, updated list of their 
[500 Greatest Albums of All Time](https://www.rollingstone.com/music/music-lists/best-albums-of-all-time-1062063/) 
for 2020.

I thought it would be a good idea, whilst at home during the pandemic, to listen to all 500 albums in 
descending order. One way to accomplish this is to scroll through the list of albums, listening to them one 
by one, but the programmer in me thought to go one better! Why not scrape the HTML so I can create a private 
document in [Google Sheets](https://www.google.com/sheets/about/), marking each of the albums as I listen to 
them and writing a comment on each one?

## Scrape using Puppeteer Sharp

I decided to scrape the HTML using [Puppeteer Sharp](), the .NET port of the popular 
[Puppeteer](https://developers.google.com/web/tools/puppeteer) Node library by Google, mostly because I'm a 
.NET developer and also because I'm already familiar with it. It's a really useful library that will enable a 
developer to *"puppeteer"* a web browser and automate a series of tasks.

Once the [Puppeteer Sharp](https://www.nuget.org/packages/puppeteersharp/) package is installed in a new 
project *(in this case a console application)*, we can begin to scrape the HTML of all of the pages 
that contain the 500 greatest albums of all time.

Let's begin by downloading the default version of Chromium onto the local machine and opening a new browser:

```csharp
// Download the Chromium revision if it does not already exist
await new BrowserFetcher().DownloadAsync(BrowserFetcher.DefaultRevision);

// Create an instance of the browser and configure launch options
Browser browser = await Puppeteer.LaunchAsync(new LaunchOptions
{
    Headless = false
});
```

We'll need a list of all of the pages that contain the 500 albums to iterate through:

```csharp
// Store the pages where the 500 greatest albums are listed in a dictionary
Dictionary<string, string> pages = new Dictionary<string, string>()
{
    { "500 - 451", "https://www.rollingstone.com/music/music-lists/best-albums-of-all-time-1062063/arcade-fire-%ef%bb%bffuneral-1062733/" },
    { "450 - 401", "https://www.rollingstone.com/music/music-lists/best-albums-of-all-time-1062063/linda-mccartney-and-paul-ram-1062783/" },
    { "400 - 351", "https://www.rollingstone.com/music/music-lists/best-albums-of-all-time-1062063/the-go-gos-beauty-and-the-beat-1062833/" },
    { "350 - 301", "https://www.rollingstone.com/music/music-lists/best-albums-of-all-time-1062063/stevie-wonder-music-of-my-mind-2-1062883/" },
    { "300 - 251", "https://www.rollingstone.com/music/music-lists/best-albums-of-all-time-1062063/shania-twain-come-on-over-1062933/" },
    { "250 - 201", "https://www.rollingstone.com/music/music-lists/best-albums-of-all-time-1062063/buzzcocks-singles-going-steady-2-1062983" },
    { "200 - 151", "https://www.rollingstone.com/music/music-lists/best-albums-of-all-time-1062063/sade-diamond-life-1063033/" },
    { "150 - 101", "https://www.rollingstone.com/music/music-lists/best-albums-of-all-time-1062063/bruce-springsteen-nebraska-3-1063083/" },
    { "100 - 51", "https://www.rollingstone.com/music/music-lists/best-albums-of-all-time-1062063/the-band-music-from-big-pink-2-1063133/" },
    { "50 - 1", "https://www.rollingstone.com/music/music-lists/best-albums-of-all-time-1062063/jay-z-the-blueprint-3-1063183/" }
};
```

Now that we have the list of pages in a dictionary object, we can iterate through each page, scraping the albums one by one 
*(50 per page)* using the elements in the HTML to indicate where each album and it's metadata are placed. We'll assign the 
album metadata to variables and output these to the console window for the purpose of this example:

***Please note: In this example we're using an ethical user-agent header. This is to let the website know this is a web scraping script and to provide contact information. To find out more on ethical web scraping, please visit [The Ultimate Guide To Ethical
Web Scraping](https://finddatalab.com/ethicalscraping).***

```csharp
// Iterate through the dictionary of pages
foreach (KeyValuePair<string, string> entry in pages)
{
    Console.WriteLine($"Opening page for {entry.Key}");

    // Create a new page using PuppeteerSharp (using ethical user agent header)
    Page page = await browser.NewPageAsync();
    await page.SetUserAgentAsync("Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/86.0.4240.75 Safari/537.36; Kilt and Code/www.kiltandcode.com");
    await page.GoToAsync(entry.Value);

    // Wait for the element containing the albums to finish loading and assign it to an ElementHandle
    await page.WaitForSelectorAsync(".c-gallery-vertical");
    ElementHandle elementHandle = await page.QuerySelectorAsync(".c-gallery-vertical");

    // Assign all of the albums inside the album container to an ElementHandle array
    ElementHandle[] albums = await elementHandle.QuerySelectorAllAsync(".c-gallery-vertical-album");

    // Iterate through the ElementHandle array of albums
    foreach (ElementHandle album in albums)
    {
        // Get the album number
        string number = await album.QuerySelectorAsync(".c-gallery-vertical-album__number")
            .EvaluateFunctionAsync<string>("node => node.innerText");

        // Get the album title
        string title = await album.QuerySelectorAsync(".c-gallery-vertical-album__title")
            .EvaluateFunctionAsync<string>("node => node.innerText");

        // Get the album subtitle
        string subtitle = await album.QuerySelectorAsync(".c-gallery-vertical-album__subtitle")
            .EvaluateFunctionAsync<string>("node => node.innerText");

        Console.WriteLine($"Album number: {number}\nAlbum title: \"{title}\"\nAlbum subtitle: {subtitle}\n\n");
    }

    // Close the page
    await page.CloseAsync();
}
```

Please be aware that the HTML elements *(class names, etc.)* used in this code snippet may change over time, 
and these will need to be updated accordingly if a class name or element no longer exists.

Once we're done, let's close the browser and write to the console that the scrape is complete:

```csharp
// Close the browser
await browser.CloseAsync();

Console.WriteLine("Scrape complete. Press any key to exit...");
```

{%
    include image.html
    year='2020'
    month='10'
    file='500-greatest-albums-in-terminal.png'
    alt='500 greatest albums in a terminal window'
%}

While the official list of the 
[500 Greatest Albums of All Time](https://www.rollingstone.com/music/music-lists/best-albums-of-all-time-1062063/) 
should be the first place you visit for this information, I hope this article is a useful resource for how to use 
[Puppeteer Sharp](https://www.puppeteersharp.com/) to scrape the list of albums with the aim of listening through all of 
them, one by one in descending order.
