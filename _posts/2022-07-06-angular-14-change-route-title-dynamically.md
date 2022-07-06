---
layout: post
title: "How to change page title with routing in Angular application?"
tags: [angular, angular-14]
excerpt: ""
---

Here is how you can change page title with routing in Angular 14 thanks to the [latest angular 14 feature](https://efficientcoder.net/angular-14-release-features/).

Angular v14 provides a built-in strategy service for getting the title from the route, and setting the browser's page title dynamically as follows:


```ts
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { AuthComponent } from './auth.component';
import { ContactComponent } from './contact.component';

const routes: Routes = [
  {
    path: 'auth',
    component: AuthComponent,
    title: "Auth page"
  },
  {
    path: 'contact',
    component: ContactComponent,
    title: "Contact"
  }
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```
