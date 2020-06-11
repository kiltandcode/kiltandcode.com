---
title: "25+ Visual Studio Code Extensions That Are Worth Installing"
date: 2020-02-02 8:57:00 +00:00
author: "Steven McLintock"
layout: post
image: /assets/img/2020/02/vscode-extensions.png
category: vscode
---

Visual Studio Code is a code editor that has me falling head over heels for full stack development all over again. Visual Studio's younger sibling, [Visual Studio Code](https://code.visualstudio.com/) is built using the [Electron](https://www.electronjs.org/) framework, is [open source](https://github.com/microsoft/vscode) and was recently announced as the [default](https://developers.facebook.com/blog/post/2019/11/19/facebook-microsoft-partnering-remote-development/) development environment at Facebook. It's no surprise that it's now the most popular development environment according to the [Stack Overflow Developer Survey 2019](https://insights.stackoverflow.com/survey/2019). Controversially, it's *predicted* to be on track to [replace](https://movingfulcrum.com/visual-studio-code-will-replace-visual-studio/) Visual Studio one day.

{%
    include image.html
    year='2020'
    month='02'
    file='stackoverflow-survey-vscode.png'
    alt='VSCode in Stack Overflow Developer Survey 2019'
%}

I was hesitant to install Visual Studio Code in the beginning. If it's built on Electron, it's essentially a web app, right? HTML, CSS and JavaScript under the hood. How could that be worth using over the heavyweight that is Visual Studio? Where as Visual Studio is known as the *'heavyweight'* for Microsoft centric development, Visual Studio Code couldn't be more different. Visual Studio Code adopts the slimline approach of [.NET Core](https://github.com/dotnet/core); only including the essentials and enabling the user to tailor their experience to specific requirements.

## Extensions for Visual Studio Code

With the help of the developer community as well as the [Visual Studio Marketplace](https://marketplace.visualstudio.com/VSCode), a user can install extensions to enhance the features available in Visual Studio Code. In addition to features, extensions can also be useful for enabling Visual Studio Code to support programming languages such as **TypeScript**, **Python**, **Java**, **Ruby**, **Go**, **PHP**, **Objective-C** and many more.

{%
    include image.html
    year='2020'
    month='02'
    file='vscode-extensions.png'
    alt='Visual Studio Code extensions'
%}

As much as I was hesitant to use Visual Studio Code, I was reluctant to install too many extensions. Why? I was happy to use one or two to get the job done and didn't want to bloat my code editor with extensions I never used. What changed my mind? I was investigating a defect in TypeScript and after a few hours I was tearing my hair out! I asked a kind developer on my team to pull the feature-branch to their development environment and within 2 minutes they had identified the issue. I was amazed! One of their installed extensions *([TSLint](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-typescript-tslint-plugin))* had highlighted the issue and suggested a quick fix.

As I began exploring more extensions, I thought I'd share those I came across that are worth installing and may save you those crucial few hours in your development time.

***Please note:** If you have any suggestions for an extension that is missing from this list, please let me know at [@kiltandcode](https://twitter.com/kiltandcode) and I'll update this article.*

### JavaScript, TypeScript & Node

<table class="table">
  <thead>
    <tr>
      <th scope="col">Icon</th>
      <th scope="col">Title</th>
      <th scope="col">Author</th>
      <th scope="col">Description</th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td markdown="span">![ESLint]({{ site.baseurl }}/assets/img/2020/02/ext/eslint.jpg)</td>
      <td markdown="span">[ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)</td>
      <td markdown="span">[Dirk Baeumer](https://marketplace.visualstudio.com/publishers/dbaeumer)</td>
      <td>Integrates ESLint JavaScript into VS Code</td>
    </tr>

    <tr>
      <td markdown="span">![JavaScript Booster]({{ site.baseurl }}/assets/img/2020/02/ext/javascriptbooster.jpg)</td>
      <td markdown="span">[JavaScript Booster](https://marketplace.visualstudio.com/items?itemName=sburg.vscode-javascript-booster)</td>
      <td markdown="span">[Stephan Burguchev](https://marketplace.visualstudio.com/publishers/sburg)</td>
      <td>Boost your productivity with advanced JavaScript refactorings and commands</td>
    </tr>

    <tr>
      <td markdown="span">![JS Refactor]({{ site.baseurl }}/assets/img/2020/02/ext/jsrefactor.jpg)</td>
      <td markdown="span">[JS Refactor](https://marketplace.visualstudio.com/items?itemName=cmstead.jsrefactor)</td>
      <td markdown="span">[Chris Stead](https://marketplace.visualstudio.com/publishers/cmstead)</td>
      <td>Automated refactoring tools to smooth your development workflow</td>
    </tr>

    <tr>
      <td markdown="span">![node-readme]({{ site.baseurl }}/assets/img/2020/02/ext/node-readme.jpg)</td>
      <td markdown="span">[node-readme](https://marketplace.visualstudio.com/items?itemName=bengreenier.vscode-node-readme)</td>
      <td markdown="span">[bengreenier](https://marketplace.visualstudio.com/publishers/bengreenier)</td>
      <td>A vscode extension to view javascript module documentation in editor</td>
    </tr>

    <tr>
      <td markdown="span">![npm Intellisense]({{ site.baseurl }}/assets/img/2020/02/ext/npmintellisense.jpg)</td>
      <td markdown="span">[npm Intellisense](https://marketplace.visualstudio.com/items?itemName=christian-kohler.npm-intellisense)</td>
      <td markdown="span">[Christian Kohler](https://marketplace.visualstudio.com/publishers/christian-kohler)</td>
      <td>Visual Studio Code plugin that autocompletes npm modules in import statements</td>
    </tr>

    <tr>
      <td markdown="span">![Debugger for Chrome]({{ site.baseurl }}/assets/img/2020/02/ext/debuggerforchrome.jpg)</td>
      <td markdown="span">[Debugger for Chrome](https://marketplace.visualstudio.com/items?itemName=msjsdiag.debugger-for-chrome)</td>
      <td markdown="span">[Microsoft](https://marketplace.visualstudio.com/publishers/Microsoft)</td>
      <td>Debug your JavaScript code in the Chrome browser, or any other target that supports the Chrome Debugger protocol</td>
    </tr>

    <tr>
      <td markdown="span">![Debugger for Firefox]({{ site.baseurl }}/assets/img/2020/02/ext/debuggerforfirefox.jpg)</td>
      <td markdown="span">[Debugger for Firefox](https://marketplace.visualstudio.com/items?itemName=firefox-devtools.vscode-firefox-debug)</td>
      <td markdown="span">[Firefox DevTools](https://marketplace.visualstudio.com/publishers/firefox-devtools)</td>
      <td>Debug your web application or browser extension in Firefox</td>
    </tr>
  </tbody>
</table>

### Visual Enhancements

<table class="table">
  <thead>
    <tr>
      <th scope="col">Icon</th>
      <th scope="col">Title</th>
      <th scope="col">Author</th>
      <th scope="col">Description</th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td markdown="span">![Bracket Pair Colorizer 2]({{ site.baseurl }}/assets/img/2020/02/ext/bracketpaircolorzer2.jpg)</td>
      <td markdown="span">[Bracket Pair Colorizer 2](https://marketplace.visualstudio.com/items?itemName=CoenraadS.bracket-pair-colorizer-2)</td>
      <td markdown="span">[CoenraadS](https://marketplace.visualstudio.com/publishers/CoenraadS)</td>
      <td>A customizable extension for colorizing matching brackets</td>
    </tr>

    <tr>
      <td markdown="span">![Indenticator]({{ site.baseurl }}/assets/img/2020/02/ext/indenticator.jpg)</td>
      <td markdown="span">[Indenticator](https://marketplace.visualstudio.com/items?itemName=SirTori.indenticator)</td>
      <td markdown="span">[SirTori](https://marketplace.visualstudio.com/publishers/SirTori)</td>
      <td>Highlights your current indent depth</td>
    </tr>

    <tr>
      <td markdown="span">![Path Intellisense]({{ site.baseurl }}/assets/img/2020/02/ext/pathintellisense.jpg)</td>
      <td markdown="span">[Path Intellisense](https://marketplace.visualstudio.com/items?itemName=christian-kohler.path-intellisense)</td>
      <td markdown="span">[Christian Kohler](https://marketplace.visualstudio.com/publishers/christian-kohler)</td>
      <td>Visual Studio Code plugin that autocompletes filenames</td>
    </tr>

    <tr>
      <td markdown="span">![XML]({{ site.baseurl }}/assets/img/2020/02/ext/xml.jpg)</td>
      <td markdown="span">[XML](https://marketplace.visualstudio.com/items?itemName=redhat.vscode-xml)</td>
      <td markdown="span">[Red Hat](https://marketplace.visualstudio.com/publishers/redhat)</td>
      <td>XML Language Support by Red Hat</td>
    </tr>

    <tr>
      <td markdown="span">![Beautify]({{ site.baseurl }}/assets/img/2020/02/ext/beautify.jpg)</td>
      <td markdown="span">[Beautify](https://marketplace.visualstudio.com/items?itemName=HookyQR.beautify)</td>
      <td markdown="span">[HookyQR](https://marketplace.visualstudio.com/publishers/HookyQR)</td>
      <td>Beautify javascript, JSON, CSS, Sass, and HTML in Visual Studio Code</td>
    </tr>

    <tr>
      <td markdown="span">![vscode-icons]({{ site.baseurl }}/assets/img/2020/02/ext/vscode-icons.jpg)</td>
      <td markdown="span">[vscode-icons](https://marketplace.visualstudio.com/items?itemName=vscode-icons-team.vscode-icons)</td>
      <td markdown="span">[VSCode Icons Team](https://marketplace.visualstudio.com/publishers/vscode-icons-team)</td>
      <td>Add useful icons based on file types to the Explorer view</td>
    </tr>
  </tbody>
</table>

### Git

<table class="table">
  <thead>
    <tr>
      <th scope="col">Icon</th>
      <th scope="col">Title</th>
      <th scope="col">Author</th>
      <th scope="col">Description</th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td markdown="span">![GitLens - Git supercharged]({{ site.baseurl }}/assets/img/2020/02/ext/gitlens.jpg)</td>
      <td markdown="span">[GitLens - Git supercharged](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens)</td>
      <td markdown="span">[Eric Amodio](https://marketplace.visualstudio.com/publishers/eamodio)</td>
      <td>Visualize code authorship at a glance via Git blame annotations and code lens, seamlessly navigate and explore Git repositories, gain valuable insights via powerful comparison commands, and so much more</td>
    </tr>

    <tr>
      <td markdown="span">![GitHub Pull Requests]({{ site.baseurl }}/assets/img/2020/02/ext/githubpullrequests.jpg)</td>
      <td markdown="span">[GitHub Pull Requests](https://marketplace.visualstudio.com/items?itemName=GitHub.vscode-pull-request-github)</td>
      <td markdown="span">[GitHub](https://marketplace.visualstudio.com/publishers/GitHub)</td>
      <td>This extension allows you to review and manage GitHub pull requests in Visual Studio Code</td>
    </tr>
  </tbody>
</table>

### Documentation

<table class="table">
  <thead>
    <tr>
      <th scope="col">Icon</th>
      <th scope="col">Title</th>
      <th scope="col">Author</th>
      <th scope="col">Description</th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td markdown="span"></td>
      <td markdown="span">[Capitalize](https://marketplace.visualstudio.com/items?itemName=viablelab.capitalize)</td>
      <td markdown="span">[Viable lab](https://marketplace.visualstudio.com/publishers/viablelab)</td>
      <td>Capitalize titles according to The Chicago Manual of Style</td>
    </tr>

    <tr>
      <td markdown="span">![Markdown All in One]({{ site.baseurl }}/assets/img/2020/02/ext/markdownallinone.jpg)</td>
      <td markdown="span">[Markdown All in One](https://marketplace.visualstudio.com/items?itemName=yzhang.markdown-all-in-one)</td>
      <td markdown="span">[Yu Zhang](https://marketplace.visualstudio.com/publishers/yzhang)</td>
      <td>All you need to write Markdown (keyboard shortcuts, table of contents, auto preview and more)</td>
    </tr>

    <tr>
      <td markdown="span">![Code Spell Checker]({{ site.baseurl }}/assets/img/2020/02/ext/codespellchecker.jpg)</td>
      <td markdown="span">[Code Spell Checker](https://marketplace.visualstudio.com/items?itemName=streetsidesoftware.code-spell-checker)</td>
      <td markdown="span">[Street Side Software](https://marketplace.visualstudio.com/publishers/streetsidesoftware)</td>
      <td>Spelling checker for source code</td>
    </tr>

    <tr>
      <td markdown="span">![Word Count]({{ site.baseurl }}/assets/img/2020/02/ext/wordcount.jpg)</td>
      <td markdown="span">[Word Count](https://marketplace.visualstudio.com/items?itemName=ms-vscode.wordcount)</td>
      <td markdown="span">[Microsoft](https://marketplace.visualstudio.com/publishers/Microsoft)</td>
      <td>Word count for Markdown documents</td>
    </tr>

    <tr>
      <td markdown="span">![Paste Image]({{ site.baseurl }}/assets/img/2020/02/ext/pasteimage.jpg)</td>
      <td markdown="span">[Paste Image](https://marketplace.visualstudio.com/items?itemName=mushan.vscode-paste-image)</td>
      <td markdown="span">[mushan](https://marketplace.visualstudio.com/publishers/mushan)</td>
      <td>Paste image directly from clipboard to Markdown</td>
    </tr>
  </tbody>
</table>

### Utilities

<table class="table">
  <thead>
    <tr>
      <th scope="col">Icon</th>
      <th scope="col">Title</th>
      <th scope="col">Author</th>
      <th scope="col">Description</th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td markdown="span">![GistPad]({{ site.baseurl }}/assets/img/2020/02/ext/gistpad.jpg)</td>
      <td markdown="span">[GistPad](https://marketplace.visualstudio.com/items?itemName=vsls-contrib.gistfs)</td>
      <td markdown="span">[Live Share Contrib](https://marketplace.visualstudio.com/publishers/vsls-contrib)</td>
      <td>VS Code extension for managing and sharing code snippets, notes and interactive samples using GitHub Gists</td>
    </tr>

    <tr>
      <td markdown="span">![Todo Plus]({{ site.baseurl }}/assets/img/2020/02/ext/todoplus.jpg)</td>
      <td markdown="span">[Todo Plus](https://marketplace.visualstudio.com/items?itemName=azad-ratzki.todo-plus)</td>
      <td markdown="span">[Azad Ratzki](https://marketplace.visualstudio.com/publishers/azad-ratzki)</td>
      <td>Create and manage TODO comments with enhanced functionality</td>
    </tr>

    <tr>
      <td markdown="span">![Partial Diff]({{ site.baseurl }}/assets/img/2020/02/ext/partialdiff.jpg)</td>
      <td markdown="span">[Partial Diff](https://marketplace.visualstudio.com/items?itemName=ryu1kn.partial-diff)</td>
      <td markdown="span">[Ryuichi Inagaki](https://marketplace.visualstudio.com/publishers/ryu1kn)</td>
      <td>Compare (diff) text selections within a file, across files, or to the clipboard</td>
    </tr>

    <tr>
      <td markdown="span">![Permute Lines]({{ site.baseurl }}/assets/img/2020/02/ext/permutelines.jpg)</td>
      <td markdown="span">[Permute Lines](https://marketplace.visualstudio.com/items?itemName=earshinov.permute-lines)</td>
      <td markdown="span">[earshinov](https://marketplace.visualstudio.com/publishers/earshinov)</td>
      <td>Reverse, Unique, Shuffle lines as in Sublime Text</td>
    </tr>
  </tbody>
</table>

### Microsoft

<table class="table">
  <thead>
    <tr>
      <th scope="col">Icon</th>
      <th scope="col">Title</th>
      <th scope="col">Author</th>
      <th scope="col">Description</th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td markdown="span">![PowerShell]({{ site.baseurl }}/assets/img/2020/02/ext/powershell.jpg)</td>
      <td markdown="span">[PowerShell](https://marketplace.visualstudio.com/items?itemName=ms-vscode.PowerShell)</td>
      <td markdown="span">[Microsoft](https://marketplace.visualstudio.com/publishers/Microsoft)</td>
      <td>Develop PowerShell scripts in Visual Studio Code!</td>
    </tr>

    <tr>
      <td markdown="span">![SQL Server (mssql)]({{ site.baseurl }}/assets/img/2020/02/ext/sqlserver.jpg)</td>
      <td markdown="span">[SQL Server (mssql)](https://marketplace.visualstudio.com/items?itemName=ms-mssql.mssql)</td>
      <td markdown="span">[Microsoft](https://marketplace.visualstudio.com/publishers/Microsoft)</td>
      <td>Develop Microsoft SQL Server, Azure SQL Database and SQL Data Warehouse everywhere</td>
    </tr>

    <tr>
      <td markdown="span">![IIS Express]({{ site.baseurl }}/assets/img/2020/02/ext/iisexpress.jpg)</td>
      <td markdown="span">[IIS Express](https://marketplace.visualstudio.com/items?itemName=warren-buckley.iis-express)</td>
      <td markdown="span">[Microsoft](https://marketplace.visualstudio.com/publishers/Microsoft)</td>
      <td>This allows you to run the current folder as a website in IIS Express</td>
    </tr>
  </tbody>
</table>