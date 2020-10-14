---
title: "Scrape the 500 Greatest Albums of All Time using Puppeteer Sharp"
date: 2020-10-14 21:00:00 +00:00
author: "Steven McLintock"
layout: post
image: /assets/img/2019/04/puppeteer-logo.png
category: puppeteer
---

{%
    include image-lead.html
    year='2020'
    month='06'
    file='puppeteer-logo.png'
    alt='Puppeteer logo'
%}

As a keen Spotify user that is always in search of new albums and artists to listen to, 
I was particularly interested when I heard that [Rolling Stone](https://www.rollingstone.com/) magazine 
announced a new, updated list of their 
[500 Greatest Albums of All Time](https://www.rollingstone.com/music/music-lists/best-albums-of-all-time-1062063/) for 2020.

I thought it would be a good idea, whilst working at home during the pandemic, to listen to all 500 albums in 
descending order, from the worst to the best. One way of accomplishing this is to scroll through the list of 
albums, listening to them one by one, but the programmer in me thought to do one better! Why not scrape the list 
so I can create a document in Google Sheets, marking each album as I listen to it and writing a comment on if I 
liked it or not?
