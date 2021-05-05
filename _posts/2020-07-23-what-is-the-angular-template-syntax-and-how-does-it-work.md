---
title: "What Is the Angular Template Syntax and How Does It Work?"
date: 2020-07-23 22:00:00 +00:00
author: steven
layout: post
image: /assets/img/2020/06/angular-logo.png
icon: angular
tags: angular
---

Have you ever looked for a piece of HTML in the code of an Angular app and found something similar to ***&lt;ng-container *ngTemplateOutlet="navBarTemplate"&gt;&lt;/ng-container&gt;*** instead?

It turns out the [template syntax](https://angular.io/guide/template-syntax) in Angular can be used to store a chunk of HTML within a *"template"* that can be easily called upon elsewhere in the codebase. This is particularly useful when you want to keep the code in your components clean and tidy.

It's important to note that the Angular template syntax does not support the ***&lt;script&gt;*** tag to eliminate the risk of script injection attacks. Any instances of this tag will be ignored and a warning will be outputted to the browser console.

The code snippet below will create a template using the template syntax and use it to store a chunk of HTML that we can call upon elsewhere:

**app.component.html**

```html
<ng-container *ngTemplateOutlet="navBarTemplate"></ng-container>

<ng-template #navBarTemplate>
  <nav class="navbar navbar-expand-lg navbar-dark bg-dark mb-4">
    <div class="container">
      <a class="navbar-brand" href="#">My To Dos App</a>
      <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarsExample07" aria-controls="navbarsExample07" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
  
      <div class="collapse navbar-collapse" id="navbarsExample07">
        <ul class="navbar-nav mr-auto">
          <li class="nav-item active">
            <a class="nav-link" href="#">Home <span class="sr-only">(current)</span></a>
          </li>
          <li class="nav-item">
            <a class="nav-link" href="#">Link</a>
          </li>
          <li class="nav-item">
            <a class="nav-link disabled" href="#" tabindex="-1" aria-disabled="true">Disabled</a>
          </li>
          <li class="nav-item dropdown">
            <a class="nav-link dropdown-toggle" href="#" id="dropdown07" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">Dropdown</a>
            <div class="dropdown-menu" aria-labelledby="dropdown07">
              <a class="dropdown-item" href="#">Action</a>
              <a class="dropdown-item" href="#">Another action</a>
              <a class="dropdown-item" href="#">Something else here</a>
            </div>
          </li>
        </ul>
      </div>
    </div>
  </nav>
</ng-template>
```

## Using Context in Angular Template Syntax

We can also give a template context in case we want to reference it in multiple areas of the codebase. For example, we could pass in a different title to a navigation bar depending on the area of the app it's being used.

You can also use the **implicit** option to define a default value to be used in the context. In the example below, the title of the navigation bar is the implicit default value. There is also a boolean value *(isNavEnabled)* being passed into the template to show or hide the navigation links:

### app.component.ts:

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  navBarContext = { $implicit: 'My To Dos App', isNavEnabled: false };
}
```

### app.component.html:

```html
<ng-container *ngTemplateOutlet="navBarTemplate2; context: navBarContext"></ng-container>

<ng-template #navBarTemplate2 let-appTitle let-navEnabled="isNavEnabled">
  <nav class="navbar navbar-expand-lg navbar-dark bg-dark mb-4">
    <div class="container">
      <a class="navbar-brand" href="#">{% raw %}{{ appTitle }}{% endraw %}</a>
      <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarsExample07" aria-controls="navbarsExample07" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
      
      <div *ngIf="navEnabled" class="collapse navbar-collapse" id="navbarsExample07">
        <ul class="navbar-nav mr-auto">
          <li class="nav-item active">
            <a class="nav-link" href="#">Home <span class="sr-only">(current)</span></a>
          </li>
          <li class="nav-item">
            <a class="nav-link" href="#">Link</a>
          </li>
          <li class="nav-item">
            <a class="nav-link disabled" href="#" tabindex="-1" aria-disabled="true">Disabled</a>
          </li>
          <li class="nav-item dropdown">
            <a class="nav-link dropdown-toggle" href="#" id="dropdown07" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">Dropdown</a>
            <div class="dropdown-menu" aria-labelledby="dropdown07">
              <a class="dropdown-item" href="#">Action</a>
              <a class="dropdown-item" href="#">Another action</a>
              <a class="dropdown-item" href="#">Something else here</a>
            </div>
          </li>
        </ul>
      </div>
    </div>
  </nav>
</ng-template>
```

I hope you have found this article helpful and you're now able to use the Angular template syntax in your codebase for cleaner and tidier code.
