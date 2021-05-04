---
title: "Show Validation Error Messages for Reactive Forms in Angular 9"
date: 2020-08-13 21:00:00 +00:00
author: steven
layout: post
image: /assets/img/2020/06/angular-logo.png
icon: angular
tags: angular
---

In Angular there are two different types of forms for a user to input data: **reactive** and 
**template-driven**. I personally prefer using reactive forms as they seem to follow the 
separation of concerns principle, where the core functionality is in the component and the 
presentation layer is stored in the template.

We can define a reactive form within a component and check if the form is valid, but if there's an error, 
how do we indicate this to the user in a manner they can understand and rectify accordingly?

In this example, I'll define a reactive form with one text field. We'll configure this field to have a 
few validation rules before the user can submit the form. These will be to set the field 
as **required**, it's value to be of a **minimum character length** of 3 and have a 
**maximum character length** of 30.

The reactive form defined in the component:

```typescript
this.formGroup = this.formBuilder.group({
    toDo: ['', [Validators.required, Validators.minLength(3), Validators.maxLength(30)]]
});
```

## Show the Field is Required Using the Label

Using data binding in Angular, we can dynamically change the label to indicate the field is 
required. This will at least let the user know that the field is not optional:

{%
    include image.html
    year='2020'
    month='08'
    file='reactive-forms-label-required.png'
    alt='Label for required field for reactive form in Angular'
%}

```html
<label for="toDo">
    Add New To Do
    <span 
        class="text-danger" 
        *ngIf="formGroup.get('toDo').errors && 
            formGroup.get('toDo').hasError('required')"
    >*</span>
</label>
```

## Show the Validation State Using the Field

We can also use data binding to apply the classes *'valid'* or *'invalid'* to the field to indicate 
if there is a validation error *(or if the field is valid)*:

{%
    include image.html
    year='2020'
    month='08'
    file='reactive-forms-valid-field.png'
    alt='Valid field in reactive form in Angular'
%}

```html
<input 
    id="toDo" 
    type="text" 
    class="form-control" 
    placeholder="What is it you want to do?"
    formControlName="toDo"
    [class.valid]="formGroup.get('toDo').valid && 
        (formGroup.get('toDo').dirty || formGroup.get('toDo').touched)"
    [class.invalid]="formGroup.get('toDo').invalid && 
        (formGroup.get('toDo').dirty || formGroup.get('toDo').touched)" />
```

## Show Each of the Validation Error Messages

We'll need to clearly indicate to the user if a field is required, or if the user has not met 
the validation rules set for a minimum character length or exceeded a maximum character 
length. You could also add to this to include other validation rules such as email addresses or 
forbidden names. Let's show each error message using data binding and HTML:

**Required error message:**

{%
    include image.html
    year='2020'
    month='08'
    file='reactive-forms-required.png'
    alt='Required field in reactive form in Angular'
%}

**Minimum length error message:**

{%
    include image.html
    year='2020'
    month='08'
    file='reactive-forms-min-length.png'
    alt='Minimum length error in reactive form in Angular'
%}

**Maximum length error message:**

{%
    include image.html
    year='2020'
    month='08'
    file='reactive-forms-maxlength.png'
    alt='Maximum length error in reactive form in Angular'
%}

```html
<div *ngIf="formGroup.get('toDo').invalid && 
        formGroup.get('toDo').errors && 
        (formGroup.get('toDo').dirty || formGroup.get('toDo').touched)">
    <small class="text-danger"
        *ngIf="formGroup.get('toDo').hasError('required')">
        This field is required.
    </small>
    <small class="text-danger"
        *ngIf="formGroup.get('toDo').hasError('minlength')">
        The minimum length for this field is {% raw %}{{formGroup.get('toDo').errors.minlength.requiredLength}}{% endraw %} characters.
    </small>
    <small class="text-danger"
        *ngIf="formGroup.get('toDo').hasError('maxlength')">
        The maximum length for this field is {% raw %}{{formGroup.get('toDo').errors.maxlength.requiredLength}}{% endraw %} characters.
    </small>
</div>
```

## Disable the Submit Button

Finally, we can disable the submit button if the form is invalid:

```html
<button class="btn btn-primary" [disabled]="formGroup.invalid">Add To Do</button>
```

Instead of duplicating this code for each field and for all forms, a better approach might be 
to abstract this into a separate component that can be reused across the 
codebase. However, this should hopefully be a good example of how to show validation error 
messages to the user in a reactive form.