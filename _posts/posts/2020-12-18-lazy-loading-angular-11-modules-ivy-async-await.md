---
layout: post
title:  "Lazy-Loading Angular 11 Modules (Ivy and Async Await)"
---

Throughout this tutorial you’ll see by example how to Lazy Load an Angular 11 module. Lazy loading means that our code isn’t downloaded by the browser  _until it’s needed_.

## What is Lazy Loading?

For instance, if I login to  `/admin`  I would get a “chunk” of JavaScript code specifically for the Admin dashboard. Similarly, if I load  `/shop`  I would expect another “chunk” of JavaScript specifically for just the Shop!

If I visited the Admin panel and navigated to Shop, the code related to the Shop would be lazy loaded and “injected” into the browser (in simple terms) for us to use the Shop module.

After compiling and deploying our Angular 11 app, we could have a JS file per lazy loaded module, for example:

```
main.chunk.js // loaded everywhere
shop.chunk.js // lazy module
admin.chunk.js // lazy module

```

The best thing is that when we visit  `/admin`, there is  _no_  code downloaded that’s part of our Shop, and vice versa. This keeps bundle sizes small, and code efficiently performant.

With this in mind, let’s learn how to lazy load any module with Angular 11! I also want to show you a newer approach that uses  [async await](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous/Async_await).

## Lazy Loading an Angular 11 Module

Back in TypeScript 2.4 the  dynamic import syntax was introduced, this became the defacto way of importing TypeScript modules on the fly (so they weren’t bundled with our code).

Angular 11 is built with TypeScript - so what did that mean? Angular 11 had to change its approach to lazy loading a module - as this was done via a “magic string” beforehand - to the new dynamic import syntax (which makes so much more sense)!

## Create an Angular 11 Module to Lazy Load

First, we’ll need to create a lazy module in our app, which will use  `RouterModule.forChild(routes)`  to define a child module:

```
// lazy.module.ts
import { NgModule } from '@angular/core';
import { Routes, RouterModule } from '@angular/router';

import { LazyComponent } from './lazy/lazy.component';

const routes: Routes = [
  {
    path: '',
    component: LazyComponent,
  },
];

@NgModule({
  declarations: [LazyComponent],
  imports: [RouterModule.forChild(routes)],
})
export class LazyModule {}

```

Using  `path: ''`  allows us to define which route we’ll hit for our  `LazyModule`  to be created, we’ll see this momentarily.

At this point, just because our module is “called” lazy, it’s not lazy yet. We’ll need to add this to our root app module, and define the routes we’d like to setup.

## Declare the Lazy Load Angular 11 Routes

The way we lazy load Angular 11 modules in v9 and above (with Angular 11 Ivy!) is through the route declarations!

Currently, the  [Angular 11 documentation suggests](https://angular.io/guide/lazy-loading-ngmodules)  a Promise-based syntax for declaring which routes to lazy load, which we add to a  `loadChildren`  property on the routes:

```
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { Routes, RouterModule } from '@angular/router';

import { AppComponent } from './app.component';

const routes: Routes = [
  {
    path: 'lazy', // visit `/lazy` to load LazyModule
    loadChildren: () => import('./lazy/lazy.module').then(m => m.LazyModule),
  },
  { path: '', pathMatch: 'full', redirectTo: 'lazy' },
];

@NgModule({
  declarations: [AppComponent],
  imports: [BrowserModule, RouterModule.forRoot(routes)],
  providers: [],
  bootstrap: [AppComponent],
})
export class AppModule {}

```

But Angular 11 isn’t about Promises, so I find the  `.then(m => m.LazyModule)`  a bit of an eyesore, it just feels wrong in the codebase - can we do better?

## Using async await with Angular 11 

With the addition of async await in TypeScript, we can use the syntactic sugar to clean things up a little!

Changing over our Promise-based routing implementation to using the newer async await syntax looks far nicer.

We can omit the  `.then()`  altogether and use the await the  _return value_  from  `await import(...)`  and reference  `.LazyModule`  directly:

```
const routes: Routes = [
  {
    path: 'lazy',
    loadChildren: async () => (await import('./lazy/lazy.module')).LazyModule,
  },
  { path: '', pathMatch: 'full', redirectTo: 'lazy' },
];

```

## Legacy syntax (Angular v7 and below)

Lazy loading in Angular has come a long way since the beginning, where we used magic string to denote a module to be loaded - just for fun here’s what we used to do:

```
const routes: Routes = [
  {
    path: 'lazy',
    loadChildren: './lazy/lazy.module#LazyModule',
  },
  { path: '', pathMatch: 'full', redirectTo: 'lazy' },
];

```

You can see how this could easily be prone to spelling errors and the mystical  `#`  to denote the module’s name. Now we just simply reference the object directly which is nicely inferred through TypeScript.

It’s great that Angular 11 Ivy has introduced better techniques for lazy loading that make it even more efficient for us to get setup and code splitting!
