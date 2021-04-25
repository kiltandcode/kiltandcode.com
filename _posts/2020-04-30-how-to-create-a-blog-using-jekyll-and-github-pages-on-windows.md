---
title: "How to Create a Blog Using Jekyll and GitHub Pages on Windows"
date: 2020-04-30 21:25:00 +00:00
author: "Steven McLintock"
layout: post
image: /assets/img/2020/04/jekyll-logo.png
icon: jekyll.png
tags: jekyll github-pages
---

Nearly a year ago I migrated from the popular blogging platform [WordPress](https://wordpress.org) to the static site generator [Jekyll](https://jekyllrb.com). Why am I and thousands of other bloggers deciding to move to a static site generator? It's a no-brainer! Why should we install security updates, manage plugins and tune the performance of a website so our pages load fast? Blogging should be all about the writing!

With Jekyll you don't need to worry about all of that. Jekyll has **no admin interface**, **only source files** that you can build using a CLI *(command line interface)* to create a [static website](https://www.smashingmagazine.com/2015/11/modern-static-website-generators-next-big-thing/). Simply write your blog posts in [Markdown](https://www.markdownguide.org/) and if you choose to host your website using [GitHub Pages](https://pages.github.com), you can use [Git](https://git-scm.com) to commit and push your changes to publish new posts or pages.

Jekyll is easy to install on [macOS](https://www.apple.com/macos/catalina/) and [Linux](https://www.linux.org), but there are a few extra steps required to be able to use Jekyll on Windows. This tutorial will guide you through:

* How to create a Jekyll blog on Windows
* Configuring your new Jekyll blog
* Writing posts and pages in Markdown
* Hosting your blog using GitHub Pages

## Prerequisites

In order to follow along with this tutorial, you'll need to be using a recent version of the Windows operating system. You'll also need [Visual Studio Code](https://code.visualstudio.com) *(or a similar code editor)*, [Git for Windows](https://gitforwindows.org) *(or your favorite Git client)* and a [GitHub](https://github.com) account if you would like to use GitHub Pages.

## How to Install Jekyll on Windows

Jekyll is written in Ruby as a [gem](https://rubygems.org/), so to run Jekyll on Windows we'll first need to download and install [RubyInstaller for Windows](https://rubyinstaller.org/). Make sure to download a recent **Ruby+DevKit** version and use the default options in the installation wizard. On the last step, you'll want to keep the option **"Run 'ridk install' to setup MSYS2 and development toolchain."** checked.

{%
    include image.html
    year='2020'
    month='04'
    file='rubyinstall-1.png'
    alt='Completing the Ruby+DevKit with MSYS2 Setup Wizard'
    caption='Completing the Ruby+DevKit with MSYS2 Setup Wizard'
%}

You've almost installed Ruby on Windows! Once finished the installation wizard, you should see a command prompt to setup MSYS2 and the development toolchain. Simply enter **1** in the command prompt to install the MSYS2 base installation.

{%
    include image.html
    year='2020'
    month='04'
    file='rubyinstall-2.png'
    alt='Running ridk install to setup MSYS2 and the development toolchain'
    caption='Running <em>ridk install</em> to setup MSYS2 and the development toolchain'
%}

You can now close the command prompt and open up a fresh one, because now that we have Ruby installed, it's time to install Jekyll! In the new command prompt, execute the command:

```terminal
gem install jekyll bundler
```

{%
    include image.html
    year='2020'
    month='04'
    file='jekyllinstall-1.png'
    alt='Install the Ruby gems jekyll and bundler'
    caption='Install the Ruby gems <em>jekyll</em> and <em>bundler</em>'
%}

Let's confirm Jekyll is installed on Windows by outputting the version of Jekyll on our machine. To do this, run the command:

```terminal
jekyll -v
```

{%
    include image.html
    year='2020'
    month='04'
    file='jekyllinstall-2.png'
    alt='Confirm Jekyll has been installed successfully'
    caption='Confirm Jekyll has been installed successfully'
%}

Now you're ready to use Jekyll in the command line to create your new blog!

## How to Create a New Blog using Jekyll

Using Jekyll we can **use one command to create a new blog** with a default theme, post and page. Create a new folder on your PC for your Jekyll blog and use it's full path as the parameter in the following command:

```terminal
jekyll new PATH
```

{%
    include image.html
    year='2020'
    month='04'
    file='jekyll-new-command.png'
    alt='Create a new blog using the jekyll new command'
    caption='Create a new blog using the <em>jekyll new</em> command'
%}

Thats it! If you check the folder, Jekyll will have created the necessary files for your new blog; including the **default [Minima](https://github.com/jekyll/minima) theme**, an example post and page and the default configuration settings.

**Let's see how it looks!** I'm going to use the free code editor [Visual Studio Code](https://code.visualstudio.com) by [Microsoft](https://www.microsoft.com/) for the remainder of this tutorial to execute commands in a terminal *and* be able to edit the Jekyll files in the same workspace.

Open the Jekyll folder in Visual Studio Code and create a new terminal *(**Ctrl+`**)*. Now we can run the command to build and locally serve our Jekyll blog:

```terminal
jekyll serve
```

{%
    include image.html
    year='2020'
    month='04'
    file='jekyll-serve-command.png'
    alt='Run the jekyll serve command to build and locally serve your Jekyll blog'
    caption='Run the <em>jekyll serve</em> command to build and locally serve your Jekyll blog'
%}

Once Jekyll has used the source files to build the website, we can now use the server address generated by Jekyll to view our new blog locally in the browser.

{%
    include image.html
    year='2020'
    month='04'
    file='new-jekyll-site-in-browser.png'
    alt='Browse your Jekyll blog locally!'
    caption='Browse your Jekyll blog locally!'
%}

Congratulations! You now have a Jekyll blog using the default settings running locally on your PC.

## Configuration

Remember I mentioned there is no admin interface? Instead Jekyll uses the *_config.yml* file located in the root of your Jekyll directory to configure your website. The *_config.yml* file is written in [YAML](https://yaml.org/) *(YAML Ain't Markup Language)*, a human friendly data-serialization language.

### Default Settings

Below are the default settings in the *_config.yml* file. Make sure to change the default **"Your awesome title"** to the title of your blog or website, as well as your email address and an interesting description of the subject of your blog.

```yaml
title: Your awesome title
email: your-email@example.com
description: >- # this means to ignore newlines until "baseurl:"
  Write an awesome description for your new site here. You can edit this
  line in _config.yml. It will appear in your document head meta (for
  Google search results) and in your feed.xml site description.
baseurl: "" # the subpath of your site, e.g. /blog
url: "" # the base hostname & protocol for your site, e.g. http://example.com
twitter_username: jekyllrb
github_username:  jekyll

# Build settings
theme: minima
plugins:
  - jekyll-feed
```

### Baseurl and Url

The **baseurl** setting is only used if your blog or website is **not** going to sit at the root of your domain name. For example, if your Jekyll website will be at *https://myawesomeblog.com/*, leave the baseurl blank. If your blog or website will be at *http://myawesomecompany.com/blog/*, enter **/blog** as the baseurl.

The **url** setting is what Jekyll uses to determine the domain name of your website. For example, the Jekyll website you are currently on uses the domain name **https://kiltandcode.com**, so that's the url value used for this website.

### Themes and Layouts

Jekyll uses the [Minima](https://github.com/jekyll/minima) theme by default, but this can easily be changed. As is the case with many blogging platforms, there are plenty of **pre-built themes** out there if you don't fancy building your own. [Jekyll Themes](https://jekyllthemes.io) is an excellent curated directory of free *(and paid)* Jekyll themes that's definitely worth checking out!

{%
    include image.html
    year='2020'
    month='04'
    file='jekyllthemes-site.png'
    alt='Screenshot of Jekyll Themes'
    caption='<a href="https://jekyllthemes.io/">Jekyll Themes</a> - a curated directory of Jekyll themes'
%}

If you want to build your own theme, **Jekyll uses the [Liquid](https://shopify.github.io/liquid/) syntax** to be able to do things like display a list of your latest blog posts, write your own pagination, almost anything you would want a good blogging platform to be able to do.

In addition to themes, Jekyll also uses **layouts** to enable you to write *(or customize)* the [HTML](https://www.w3schools.com/html/), [CSS](https://www.w3schools.com/css/default.asp) and [JavaScript](https://www.w3schools.com/js/default.asp) used on your website. You're in control!

Check out the [themes](https://jekyllrb.com/docs/themes/) and [layouts](https://jekyllrb.com/docs/layouts/) documentation on the Jekyll website to learn more.

### Permalinks

Have you ever used WordPress, or a similar blogging platform to customize the permalink style used to link to your blog posts? Jekyll can do this as well! In my opinion, this really does demonstrate how Jekyll was built to be a great blogging platform.

Simply add one of the below to your configuration file to apply the permalink style to your blog:

*https://kiltandcode.com/jekyll/2020/04/30/how-to-create-a-blog-using-jekyll-and-github-pages-on-windows/*

```yaml
permalink: /:categories/:year/:month/:day/:title/
```

*https://kiltandcode.com/2020/04/30/how-to-create-a-blog-using-jekyll-and-github-pages-on-windows/*

```yaml
permalink: /:year/:month/:day/:title/
```

*https://kiltandcode.com/how-to-create-a-blog-using-jekyll-and-github-pages-on-windows/*

```yaml
permalink: /:title/
```

### Plugins

You'll notice in the default configuration file there is a plugin already referenced: **"jekyll-feed"**. This is just one of the many plugins available for Jekyll that can provide useful functionality to your blog. The [jekyll-feed](https://github.com/jekyll/jekyll-feed) plugin will generate an Atom *(RSS-like)* feed for readers to be able to subscribe to your blog.

To install a Jekyll plugin, you will need to add the name of the plugin in two places. The first is in the *"plugins"* section of the **_config.yml** file, and the second is in the **"Gemfile"**. The *[Gemfile](https://bundler.io/gemfile.html)* is what Ruby uses to determine the gem dependencies required to execute Ruby code.

There's also many other plugins available, such as [jekyll-seo-tag](https://github.com/jekyll/jekyll-seo-tag) that will automatically add search-optimized metadata to your HTML to give you that extra boost in the search rankings *(this is my personal favorite!)*. Just remember, GitHub Pages only supports a short list of plugins, so be sure to checkout the list of [supported plugins](https://pages.github.com/versions/) before using GitHub Pages to host your website.

## Posts

If you've made it this far, you're probably wondering when we're going to get to the good stuff! This is why I, like many, migrated to Jekyll. The simplicity of writing interesting blog posts!

### Filename Structure

In the list of files that Jekyll creates using the *jekyll new* command there is a **"_posts"** folder. This is where Jekyll will look for blog posts when it builds your website, so be sure to add any new posts in this folder so Jekyll can find them.

{%
    include image.html
    year='2020'
    month='04'
    file='post-file-in-visual-studio-code.png'
    alt='Markdown file in Visual Studio Code'
    caption='Markdown file in Visual Studio Code'
%}

When writing new blog posts, be sure to use the file format:

**YEAR-MONTH-DAY-title.MARKUP**

**YEAR** is a four digit number and **MONTH** and **DAY** are two digit numbers. So, for the blog post *"10 Reasons Why Jekyll is Awesome"* written on the 1st of April 2020, the filename would be **"2020-04-01-10-reasons-why-jekyll-is-awesome.markdown"**.

### Front Matter

While first introduced in Jekyll, front matter is used in the majority of static site generators, such as the popular [GatsbyJS](https://www.gatsbyjs.org) and [Hugo](https://gohugo.io).

It's a simple way of adding information about the post or page to the top of the document. In the below front matter you will see:

* The layout used for the document
* The title of the document
* The date the document was published
* A list of categories the document belongs to

```markdown
---
layout: post
title:  "Welcome to Jekyll!"
date:   2020-04-15 15:19:23 -0700
categories: jekyll update
---
```

**Other examples of Jekyll front matter are:**

If you would prefer a specific permalink to be used instead of the default:

```markdown
permalink: /jekyll-is-awesome/
```

If you would like Jekyll to ignore a post until it's ready to be published:

```markdown
published: false
```

### Markdown

Let's get writing! When you are ready to write your blog post, you can use [Markdown](https://www.markdownguide.org/Markdown) to add rich text formatting to your documents. Created by [John Gruber](https://twitter.com/gruber) of [Daring Fireball](https://daringfireball.net), Markdown is a markup language that will convert basic syntax to HTML.

Here are a few examples you can use in your blog posts:

#### Headers *(H1, H2, H3, H4, H5, H6)*

```markdown
# H1
## H2
### H3
#### H4
##### H5
###### H6
```

#### Bold text

```markdown
**text**
```

#### Italic text

```markdown
*text*
```

#### Hyperlinks

```markdown
[Google](https://google.com)
```

#### Images

```markdown
![Screenshot](screenshot.jpg)
```

## Pages

Jekyll also supports **pages for standalone content that isn't date based**, such as an about or contact page. You can also use the [liquid syntax](https://jekyllrb.com/docs/liquid/) to create pages with custom functionality, such as a searchable list of all your blog posts.

Similar to posts, front matter can be used to add information to your page:

```yaml
---
layout: page
title: About
permalink: /about/
---
```

**Pages are stored in the root directory** of your website as either Markdown or HTML documents. You can also add pages into sub-folders in your root directory and Jekyll will take into account the sub-folder when the site is built.

## GitHub Pages

Because Jekyll was built by [Tom Preston-Werner](https://tom.preston-werner.com), the co-founder of [GitHub](https://github.com), Jekyll works exceptionally well with [GitHub Pages](https://pages.github.com).

GitHub Pages is a static website hosting service that enables you to host a *(static)* website directly from a public GitHub repository. At the time of writing, it is available on GitHub Free, GitHub Free for organizations as well as the professional and enterprise plans.

One of the great benefits of using Jekyll with GitHub Pages is that you can write a new blog post, commit it to the GitHub repository that's being used to host your blog and **your website will be automatically built to publish the new article**. This is why Jekyll and GitHub Pages work so well together!

Let's create a new GitHub repository so we can publish our new Jekyll blog on GitHub Pages. In your GitHub account, create the repository ***username***.github.io, making sure ***username*** is the username of your GitHub account.

{%
    include image.html
    year='2020'
    month='04'
    file='github-new-repo.png'
    alt='Creating a new repository on GitHub'
    caption='Creating a new repository on GitHub'
%}

Clone the new repository to your PC using git:

```terminal
git clone https://github.com/username/username.github.io
```

Copy and paste all your Jekyll files into the new local repository on your PC and push your changes to the repository:

```terminal
git add --all
git commit -m "Initial commit of Jekyll blog"
git push -u origin master
```

That's it! You'll now be able to view your Jekyll blog at the address https://username.github.io.

## Conclusion

I hope you enjoyed learning about Jekyll and how it can be used as a powerful, yet simple blogging platform.

If you found this article useful, please let me know at [@kiltandcode](https://twitter.com/kiltandcode) or email me at [stevenmclintock@fastmail.com](mailto:stevenmclintock@fastmail.com). I look forward to reading your new Jekyll blog!