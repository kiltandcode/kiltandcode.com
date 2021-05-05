---
title: "Delete Unused Images from a Jekyll Website using Powershell"
date: 2020-06-13 19:55:00 +00:00
author: steven
layout: post
image: /assets/img/2020/06/powershell-logo.png
icon: powershell
tags: powershell jekyll
---

When I moved my website from WordPress to Jekyll, a lot of unnecessary images were transferred over that were 
increasing the size of the GitHub repository by as much as 40%.

As a Windows user, what better time to use PowerShell to automatically delete all of the unused images? If we've 
got a script to do the heavy lifting for us, we'll be able to execute it any time we need to do a spring clean.

There's a few different ways to accomplish this, but these are the steps I'll be using:

* Find the **used** images on the Jekyll website
* Find **all** of the images stored in the local */assets/* folder
* Compare the **used** images against **all** of the images to find the **unused** images
* Prompt the user before deleting the **unused** images

## Find Used Images

To find all of the **used** images I'll loop through the sitemap of the website *(/sitemap.xml)* and use PowerShell 
to detect all of the images used on each page, adding each of them to an array:

```powershell
$sitemap = Invoke-WebRequest -Uri 'http://127.0.0.1:4000/sitemap.xml'

$used = @()
([xml]$sitemap.Content).urlset.url | % {
    $page = Invoke-WebRequest -Uri $_.loc

    ($page).Images | Select src | % {
        If ($used -notcontains $_.src) { $used += $_.src }
    }
}

Write-Host $used.Length 'images found'
```

## Find All Images

To find **all** of the images for the website is slightly easier. All we 
need to do is add all of the images in the */assets/* folder into a new array. Don't forget to add a filter so 
we can ignore any CSS or other documents.

```powershell
$all = @()
Get-ChildItem -Path .\assets -Recurse -Include *.jpg, *.jpeg, *gif, *png, *tif, *tiff | % {
    $img = $_.FullName.Substring($_.FullName.LastIndexOf("\assets\")).Replace('\', '/')

    If ($all -notcontains $img) { $all += $img }
}
```

## Compare, Prompt & Delete

Finally we do a compare on the two arrays and prompt the user if they'd like to go ahead and delete all of 
the unused images:

```powershell
$unused = @()
$unused = $all | Where-Object { $_ -notin $used }

$input = Read-Host 'Delete' $unused.Length 'images? [y/n]'

If ($input -eq 'y') {
    Write-Host 'Deleting ' $unused.Length ' images...'

    $path = $(get-location).Path.Replace('\assets', '')
    $unused | % {
        $item = $path + $_.Replace('/', '\')
        Remove-Item -Path $item
    }

    Write-Host 'Operation completed.'
}

Read-Host -Prompt 'Press Enter to exit'
```

{%
    include image.html
    year='2020'
    month='06'
    file='powershell-unused-images.png'
    alt='Powershell Script'
%}

As you can see, using PowerShell is fairly straight forward to automate this process. I'm sure I could optimize 
it further, but for now it works as expected and I'm able to remove all of the unused images.