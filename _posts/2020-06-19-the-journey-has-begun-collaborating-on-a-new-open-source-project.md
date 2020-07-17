---
title: "The Journey Has Begun: Collaborating on a New Open Source Project"
date: 2020-06-19 21:00:00 +00:00
author: "Steven McLintock"
layout: post
image: /assets/img/2020/06/architecture-diagram.jpg
category: open-source
---

The journey has begun! Myself, [Davide Bellone](https://www.code4it.dev/) and [Wouter van Vegchel](https://twitter.com/wvegchel) are embarking on a new open source project together to build a fun platform for helping dev bloggers share their content and help get it out to a wider audience.

<blockquote class="twitter-tweet tw-align-center"><p lang="en" dir="ltr">I&#39;d like to create a project to help dev bloggers. There are lots of interesting blogs that do not receive enough visibility and deserve more.<br><br>I&#39;d like to use <br>- <a href="https://twitter.com/hashtag/React?src=hash&amp;ref_src=twsrc%5Etfw">#React</a> (I want to learn it)<br>- <a href="https://twitter.com/hashtag/GraphQL?src=hash&amp;ref_src=twsrc%5Etfw">#GraphQL</a><br>- <a href="https://twitter.com/hashtag/DotNet?src=hash&amp;ref_src=twsrc%5Etfw">#DotNet</a><br>- <a href="https://twitter.com/hashtag/Azure?src=hash&amp;ref_src=twsrc%5Etfw">#Azure</a><br><br>Is anyone interested? <a href="https://twitter.com/hashtag/100DaysOfCode?src=hash&amp;ref_src=twsrc%5Etfw">#100DaysOfCode</a></p>&mdash; Davide Bellone 🐧 - code4it.dev 📃📃 (@BelloneDavide) <a href="https://twitter.com/BelloneDavide/status/1267744264127217664?ref_src=twsrc%5Etfw">June 2, 2020</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

I've never collaborated on a new open source project before, so I'm particularly excited to see how it turns out! The project was initiated by [Davide Bellone](https://twitter.com/BelloneDavide) to accomplish two goals: the first is to raise awareness of new or unknown bloggers in the developer community and the second is to give us all a project where we can choose the tech stack and learn some new languages and frameworks.

## The Tech Stack

So far we're fairly confident on focusing on [Microsoft Azure](https://azure.microsoft.com/) and [.NET](https://dotnet.microsoft.com/), using [Blazor](https://dotnet.microsoft.com/apps/aspnet/web-apps/blazor) for the front-end UI and hopefully getting to use [GraphQL](https://graphql.org/) for the first time, a language we're all keen to try. We're also going to focus on [test-driven development](https://www.freecodecamp.org/news/test-driven-development-what-it-is-and-what-it-is-not-41fa6bca02a2/), a technique that I.T departments and large organizations often convey they're using, but often don't due to tight deadlines *(guilty as charged!)*.

To be clear on the premise of this new open source project, we'll be building a platform to consume blog articles written by new or unknown developers using their [RSS](https://en.wikipedia.org/wiki/RSS) or [Atom](https://en.wikipedia.org/wiki/Atom_(Web_standard)) feed, retrieving the latest content on a regular basis *(daily?)* and caching each article in a database. We'll be aiming to drive traffic to each blog by creating a front-end UI for visitors to browse new articles and visit their corresponding website to read more.

To consume the new articles we have an initial architecture diagram that involves [Azure Functions](https://azure.microsoft.com/en-us/services/functions/) to retrieve the articles, [MongoDB Atlas](https://www.mongodb.com/cloud/atlas) for data storage *(using their generous free tier)* and [Azure Service Bus](https://azure.microsoft.com/en-us/services/service-bus/) for a queue-based messaging service.

{%
    include image.html
    year='2020'
    month='06'
    file='architecture-diagram.jpg'
    alt='Architecture Diagram'
%}

Finally, we have an initial design of the front-end UI to be built in Blazor among other modern frameworks. The design will enable visitors to browse categories they're interested in, or all of the new articles in descending order.

{%
    include image.html
    year='2020'
    month='06'
    file='front-end-ui.jpg'
    alt='Front-end UI'
%}

If you would like to keep up-to-date with our development, you can follow the project on [GitHub](https://github.com/DevBlogsList) and find each of us on Twitter at [@kiltandcode](https://twitter.com/kiltandcode), [@BelloneDavide](https://twitter.com/BelloneDavide) and [@wvegchel](https://twitter.com/wvegchel).