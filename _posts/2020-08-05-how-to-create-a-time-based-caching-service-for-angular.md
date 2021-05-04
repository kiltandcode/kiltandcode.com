---
title: "How to Create a Time-Based Caching Service for Angular"
date: 2020-08-05 21:00:00 +00:00
author: steven
layout: post
image: /assets/img/2020/06/angular-logo.png
icon: angular
tags: angular
---

If you're interested in building a caching service for Angular that will store the observable for a HTTP call in cache, 
there is an excellent article by [Yury Katkov](https://medium.com/better-programming/how-to-create-a-caching-service-for-angular-bfad6cbe82b0) 
that will explain exactly how to do this using [RxJS](https://rxjs.dev/).

The solution uses the [shareReplay](https://www.learnrxjs.io/learn-rxjs/operators/multicasting/sharereplay) feature to be able to 
*'replay'* the observable so we don't need to execute the same HTTP call over and over again. If we're able to locally store the 
observable, retrieve it and replay it, we have enough to build a basic caching service for Angular!

We'll first build a basic caching service for Angular and then expand on it to implement time-based caching.

## Basic Caching Service for Angular

Using the solution in the article, I built a basic caching service for Angular that will store an observable in cache on the first HTTP call 
for an API endpoint used to retrieve a to-do item. Any subsequent HTTP call will retrieve the observable from cache instead of 
making another call to the API.

Below is how we're deciding to retrieve the to-do from cache, or alternatively retrieving the to-do using the API endpoint:

{%
    include image.html
    year='2020'
    month='08'
    file='basic-angular-caching-service.png'
    alt='Flow diagram for basic Angular cache service'
%}

Here's a component that will use this logic to implement the basic caching service:

**todo-individual.component.ts:**

```typescript
import { Component, OnInit } from '@angular/core';
import { Observable, EMPTY } from 'rxjs';
import { ToDo } from '../models/todo.model';
import { shareReplay, catchError } from 'rxjs/operators';
import { HttpClient } from '@angular/common/http';
import { FormGroup, FormBuilder } from '@angular/forms';

@Component({
  selector: 'app-todo-individual',
  templateUrl: './todo-individual.component.html',
  styleUrls: ['./todo-individual.component.css']
})
export class TodoIndividualComponent implements OnInit {
  formGroup: FormGroup;
  toDoById: ToDo;
  toDoCache: { [id: string]: Observable<ToDo> };

  constructor(
    private httpClient: HttpClient,
    private formBuilder: FormBuilder
  ) {
    this.formGroup = this.formBuilder.group({});
    this.toDoCache = {};
  }
  
  ngOnInit(): void {
    this.retrieveToDo();
  }

  retrieveToDo() {
    const TO_DO_ID_TO_RETRIEVE_FROM_API: number = 1;

    this.getToDo(TO_DO_ID_TO_RETRIEVE_FROM_API).subscribe(toDo => {
      this.toDoById = toDo;
    });
  }
  
  getToDo(toDoId: number): Observable<ToDo> {
    if (this.toDoCache[toDoId]) {
      console.log('Retrieved item from cache');
      return this.toDoCache[toDoId];
    }
    
    let observable = this.httpClient.get<ToDo>(
      `https://localhost:44393/api/ToDos/${toDoId}`).pipe(
      shareReplay(1),
      catchError(err => {
        delete this.toDoCache[toDoId];
        return EMPTY;
      }));
    
    console.log('Retrieved item from API');
    
    this.toDoCache[toDoId] = observable;
    
    return observable;
  }
}
```

## Time-Based Caching Service for Angular

That's great, but what if the response from the API *(a to-do item in this case)* changes during the lifespan of the application? If a 
user doesn't reload the application for a significant amount of time, we could potentially be serving an out-of-date response.

Below is similar to before, the difference being we're going to first determine if the item stored in cache has expired or not:

{%
    include image.html
    year='2020'
    month='08'
    file='basic-angular-time-based-caching-service.png'
    alt='Flow diagram for Angular basic cache service with expiry time limit'
%}

Let's update our component to add a few more methods to implement a time-based caching service. When we add an item to the cache, we first 
set an expiry time *(30 minutes)* and ensure we have yet to exceed the expiry time when we retrieve this item from the cache:

**timestamp-observable-cache.model.ts:**

```typescript
import { Observable } from 'rxjs';

export interface TimestampObservableCache<T> {
    expires: number;
    observable: Observable<T>;
}
```

**todo-individual.component.ts:**

```typescript
import { Component, OnInit } from '@angular/core';
import { Observable, EMPTY } from 'rxjs';
import { ToDo } from '../models/todo.model';
import { shareReplay, catchError } from 'rxjs/operators';
import { HttpClient } from '@angular/common/http';
import { FormGroup, FormBuilder } from '@angular/forms';
import { TimestampObservableCache } from '../models/timestamp-observable-cache.model';

@Component({
  selector: 'app-todo-individual',
  templateUrl: './todo-individual.component.html',
  styleUrls: ['./todo-individual.component.css']
})

export class TodoIndividualComponent implements OnInit {
  formGroup: FormGroup;
  toDoById: ToDo;
  toDoCache: { [id: string]: TimestampObservableCache<ToDo> };

  constructor(
    private httpClient: HttpClient,
    private formBuilder: FormBuilder
  ) {
    this.formGroup = this.formBuilder.group({});
    this.toDoCache = {};
  }
  
  ngOnInit(): void {
    this.retrieveToDo();
  }

  retrieveToDo() {
    const TO_DO_ID_TO_RETRIEVE_FROM_API: string = '1';

    this.getToDo(TO_DO_ID_TO_RETRIEVE_FROM_API).subscribe(toDo => {
      this.toDoById = toDo;
    });
  }
  
  getToDo(toDoId: string): Observable<ToDo> {
    if (this.getCacheItem(toDoId)) {
      console.log('Retrieved item from cache');
      return this.getCacheItem(toDoId);
    }
    
    let observable = this.httpClient.get<ToDo>(
      `https://localhost:44393/api/ToDos/${toDoId}`).pipe(
      shareReplay(1),
      catchError(err => {
        this.deleteCacheItem(toDoId);
        return EMPTY;
      }));
    
    console.log('Retrieved item from API');
    
    this.setCacheItem(toDoId, observable);
    
    return observable;
  }

  getCacheItem(key: string): Observable<ToDo> {
    let cacheItem = this.toDoCache[key];

    if (!cacheItem) {
        return null;
    }

    // delete the cache item if it has expired
    if (cacheItem.expires <= Date.now()) {
        this.deleteCacheItem(key);
        return null;
    }

    return cacheItem?.observable;
  }

  setCacheItem(key: string, value: Observable<ToDo>): void {
      const EXPIRES = Date.now() + (1000 * 60 * 60) / 2;
      this.toDoCache[key] = { expires: EXPIRES, observable: value } as TimestampObservableCache<ToDo>;
  }

  deleteCacheItem(key: string) {
      delete this.toDoCache[key];
  }
}
```

This will ensure we only store an item in our Angular time-based caching service for a limited amount of time, only retrieving a fresh response 
from the API when the item has expired.