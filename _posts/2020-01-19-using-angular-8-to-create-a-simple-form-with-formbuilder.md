---
title: "Using Angular 8 to Create a Simple Form with FormBuilder"
date: 2020-01-19 11:53:00 +00:00
author: steven
layout: post
image: /assets/img/2020/01/2020-01-18 21_23_03-app.component.png
icon: angular
tags: angular
---

If you've chosen to use Angular to build the UI for your web application, or find yourself using it in an existing project, the chances are high that you're going to create a form at some point. When I was searching for how to build one myself, I found there were several contradicting examples for different versions of Angular.

This example is for Angular 8 and essentially borrows heavily from the official Google [documentation](https://angular.io/start/forms). There is one *"gotcha"* that wasn't included in the documentation that stumped me along the way, so hopefully this helps someone building their first Angular form.

## app.component.html

The HTML is fairly simple. I'm using some Bootstrap classes to help with the layout, but it's basically a regular HTML form with some Angular attribute directives on the form and input elements. Once the user is ready to submit the form, the ***onSubmit()*** method is executed with the form data.

```html
<div class="container">
  <h1>
    {% raw %}{{title}}{% endraw %}
  </h1>

  <form [formGroup]="formGroup" (ngSubmit)="onSubmit(formGroup.value)">
    <div class="form-group">
      <label for="name">Full name</label>
      <input type="text" class="form-control" id="name" 
        placeholder="Enter full name" formControlName="name" />
    </div>

    <div class="form-group">
      <label for="name">Email address</label>
      <input type="email" class="form-control" id="email" 
        placeholder="Enter email" formControlName="email" />
    </div>

    <div class="form-group form-check">
      <input type="checkbox" class="form-check-input" 
        id="terms" formControlName="terms" />
      <label class="form-check-label" for="terms">Agree to terms and conditions?</label>
    </div>

    <button class="btn btn-primary" type="submit">Submit Form</button>
  </form>
</div>
```

## app.component.ts

In the typescript component, we're importing the FormBuilder library and including it in the constructor of the component to be able to initiate the form. There is also the ***onSubmit()*** method that you could use to send the data in a POST or PUT request to an API. In this example we'll just assign one of the fields to a variable for debugging purposes.

```typescript
import { Component } from '@angular/core';
import { FormBuilder } from '@angular/forms';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  title = 'Register for stuff';
  formGroup;

  constructor(
    private formBuilder: FormBuilder
  ) {
    this.formGroup = this.formBuilder.group({
      name: '',
      email: '',
      terms: false
    });
  }

  onSubmit(formData) {
    var name = formData['name'];
  }
}
```

## app.module.ts

This is the *"gotcha"* that stumped me in the beginning. It is missing in the official documentation, but if you try and load the form in a web browser and receive the error message ***"Can't bind to 'formGroup' since it isn't a known property of 'form'"***, you need to include ***"ReactiveFormsModule"*** in your app module.

{%
    include image.html
    year='2020'
    month='01'
    file='2020-01-18 21_55_06-ReactiveFormsModule.png'
    alt='Missing ReactiveFormsModule error message'
%}

```typescript
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { FormsModule, ReactiveFormsModule } from '@angular/forms';
import { HttpModule } from '@angular/http';

import { AppComponent } from './app.component';

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    FormsModule,
    HttpModule,
    ReactiveFormsModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

Once you've created the HTML, typescript component and configured the app module, you should be ready to try it out!

{%
    include image.html
    year='2020'
    month='01'
    file='2020-01-18 21_24_04-AngularDemoApp.png'
    alt='Angular register form'
%}

If you set a breakpoint in the ***onSubmit()*** method, you should be able to see the form data being passed in.

{%
    include image.html
    year='2020'
    month='01'
    file='2020-01-18 21_23_03-app.component.png'
    alt='Debugging in Visual Studio Code'
%}