---
title: "My Thoughts on Microsoft Build 2020"
date: 2020-05-24 10:15:00 +00:00
author: steven
layout: post
image: /assets/img/2020/05/msbuild-session-catalog.png
icon: dotnetcore
tags: microsoft dotnet-core
---

Microsoft Build 2020 is over for another year, but for those of us that didn’t get a chance to watch the live stream, now is the perfect time to watch all of those sessions recorded over the last few days.

{%
    include image.html
    year='2020'
    month='05'
    file='msbuild-session-catalog.png'
    alt='Microsoft Build Session Catalog'
    caption='Microsoft Build Session Catalog'
%}

I thought it was interesting that Google is suggesting *"microsoft build 2020 cancelled"* as the first prediction when you search for *"microsoft build 2020"*. I suppose this is a sign of the times and it might well have been a possibility at some point. As reported by [The Verge](https://www.theverge.com/2020/3/4/21164522/microsoft-coronavirus-response-comment-employees-memo-work-from-home) as early as the beginning of March, Microsoft announced that their employees were allowed to work from home during the COVID-19 pandemic, demonstrating *(in my opinion)* their strength as a company to adapt so early on in a public health crisis. A week later, Microsoft Build 2020 became a virtual event to be enjoyed by all.

<blockquote class="twitter-tweet tw-align-center"><p lang="en" dir="ltr">Microsoft’s biggest event of the year goes virtual due to the coronavirus spread <a href="https://t.co/8spBaWqKGK">https://t.co/8spBaWqKGK</a> <a href="https://t.co/Yr8FN5QS7Y">pic.twitter.com/Yr8FN5QS7Y</a></p>&mdash; The Verge (@verge) <a href="https://twitter.com/verge/status/1238277644061487104?ref_src=twsrc%5Etfw">March 13, 2020</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

Admittedly and not surprisingly, I've not had time to watch all of the presentations by the likes of [Jeff Hollan](https://twitter.com/jeffhollan), [Scott Hanselman](https://www.hanselman.com/) and [Scott Guthrie](https://twitter.com/scottgu) *(to name a few)* - none the less I'm super excited about several of the new tools and features being released in the coming months!

## Project Tye

I currently get to help out on a solution using microservices and the [CQRS](https://martinfowler.com/bliki/CQRS.html) pattern. Our team has been searching for a solution to easily launch a local test environment for all of the APIs + UI and [Project Tye](https://github.com/dotnet/tye) might be just what we're looking for:

<blockquote class="twitter-tweet tw-align-center"><p lang="en" dir="ltr">✨ Check out our blog post on how you can get started using Project Tye - an experimental tool for <a href="https://twitter.com/hashtag/DotNet?src=hash&amp;ref_src=twsrc%5Etfw">#DotNet</a> that makes developing, testing, and deploying your microservices &amp; distributed applications easier! ✨ <a href="https://t.co/18BPXfEZcN">https://t.co/18BPXfEZcN</a> <a href="https://twitter.com/hashtag/tye?src=hash&amp;ref_src=twsrc%5Etfw">#tye</a> <a href="https://twitter.com/hashtag/microservices?src=hash&amp;ref_src=twsrc%5Etfw">#microservices</a> <a href="https://twitter.com/hashtag/cloudnative?src=hash&amp;ref_src=twsrc%5Etfw">#cloudnative</a> <a href="https://twitter.com/hashtag/kubernetes?src=hash&amp;ref_src=twsrc%5Etfw">#kubernetes</a></p>&mdash; Amiee Lo (@amiee_lo) <a href="https://twitter.com/amiee_lo/status/1263498571703173121?ref_src=twsrc%5Etfw">May 21, 2020</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

I think you need to setup your project(s) with a lot of specific configuration for it to work properly, but in theory you can enter one command *("tye run")* and [Project Tye](https://github.com/dotnet/tye) will manage the rest for you. I'm definitely keen to try it out.

## Windows Package Manager

I've always been interested in [Chocolately](https://chocolatey.org/) but to be honest, I'd never really gotten much use out of it after installing an application or two - that goes for the Windows Store as well.

<blockquote class="twitter-tweet tw-align-center"><p lang="en" dir="ltr">Microsoft’s new Windows Package Manager is already better than the Windows Store <a href="https://t.co/NZF0T1a2qy">https://t.co/NZF0T1a2qy</a> <a href="https://t.co/mGtOTU2HNJ">pic.twitter.com/mGtOTU2HNJ</a></p>&mdash; The Verge (@verge) <a href="https://twitter.com/verge/status/1263091007232638977?ref_src=twsrc%5Etfw">May 20, 2020</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

[Windows Package Manager](https://www.theverge.com/2020/5/20/21264739/microsoft-windows-package-manager-preview-download) is going to be similar to Chocolately in the sense that it'll install any application by using the command prompt *(or [Windows Terminal 1.0](https://devblogs.microsoft.com/commandline/windows-terminal-1-0/)!)* without the need to install or maintain any third-party services.

## GitHub Codespaces

How useful would it be to find an interesting repository on GitHub and instead of cloning the project to your local machine, you launch a new [Codespace](https://github.com/features/codespaces/) and have the project readily available to use in a new instance of Visual Studio Code right from within your browser.

{%
    include image.html
    year='2020'
    month='05'
    file='github-codespaces.png'
    alt='GitHub Codespaces'
    caption='GitHub Codespaces'
%}

This peaked my interest in particular as to sync local Visual Studio Code settings to a new GitHub Codespace, you'll soon be able to use the new [Settings Sync](https://code.visualstudio.com/docs/editor/settings-sync) feature that's currently in preview. I can definitely see this being really useful if you just want to quickly checkout a repository on a PC or Mac that isn't your own.

## .NET 5, 6 & Beyond

I've been using .NET Core in a few projects lately and I was interested to watch the presentation by [Scott Hunter](https://twitter.com/coolcsh) and [Scott Hanselman](https://www.hanselman.com/) on **"The Journey to One .NET"**.

{%
    include embed-video.html
    src='https://channel9.msdn.com/Events/Build/2020/BOD106/player'
%}

There is far too much to mention in terms of .NET, however a couple of notable items were [.NET Maui](https://www.infoworld.com/article/3544632/microsoft-unveils-net-maui-for-cross-platform-apps.html), the evolution of Xamarin.Forms as well as the release of [Blazor WebAssembly](https://visualstudiomagazine.com/articles/2020/05/19/blazor-webassembly-3-2.aspx).

I really like the transparency of Microsoft communicating the roadmap for .NET that's planned for future iterations over the next few years. And if one annual developer conference by Microsoft isn't enough, try two! We now have [.NET Conf 2020](https://www.dotnetconf.net/) to look forward to in November!