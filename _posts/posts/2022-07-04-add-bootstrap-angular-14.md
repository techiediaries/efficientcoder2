---
layout: post
title:  "Add Bootstrap to Angular 14 App"
date:   2022-07-04
---

In this tutorial, we'll learn how to add Bootstrap to Angular 14.

[Angular 14 is released with new features](https://efficientcoder.net/angular-14-release-features/) so we'll look at how to get started with Angular 14 and integrate it with Bootstrap 5. Following that, we'll utilize Bootstrap 5 CSS utilities to develop a responsive layout with tables, forms, buttons, cards, and jumbotrons.


[Bootstrap](https://getbootstrap.com/docs/4.4/getting-started/introduction/) is a free and open-source CSS framework for creating responsive layouts, it's mobile-first and contains ready CSS utilities for typography, forms, buttons, and navigation, etc.


There are various ways of [integrating Bootstrap 5 with your Angular 14](https://www.techiediaries.com/angular-bootstrap/) application. Let's see a possible solution by example. 

- Step 1 -- Installing Angular CLI 14
- Step 2 -- Initializing your Angular 14 Project
- Step 3 -- Installing Bootstrap 5
- Step 4 -- Creating Angular Components and Setting up Routing
- Step 5 -- Adding A Bootstrap 5 Jumbotron 
- Step 6 -- Creating an Angular Bootstrap 5 Table
- Step 7 -- Adding A Bootstrap 5 Form Component

## Step 1 -- Installing Angular CLI 14

Let’s start by installing the [latest version of Angular CLI](https://efficientcoder.net/install-angular-cli/). In your terminal, run the following command:

```bash
$ npm install -g @angular/cli
```

## Step 2 -- Initializing your Angular 14 Project

After installing Angular CLI, let’s [initialize an Angular 14 project](https://efficientcoder.net/angular-14-tutorial-environment-project-setup/) by running the following command:

```bash
$ ng new angularbootstrapexample
```


The CLI will then ask you:

```shell
Would you like to add Angular routing?
```
Press Y. 


Next, it will ask you:

```shell
Which stylesheet format would you like to use?
```

Choose “CSS”.

Next, we need to set up Angular forms.

Go to the `src/app/app.module.ts` file, import `FormsModule` from `@angular/forms`, and include it in the `imports` array as follows:

```ts
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { AppRoutingModule } from './app-routing.module';
import { FormsModule } from '@angular/forms';

/* ... */

@NgModule({
  declarations: [
  /* ... */
  ],
  imports: [
    BrowserModule,
    AppRoutingModule,
    FormsModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

## Step 3 -- Installing Bootstrap 5

After initializing your Angular 14 project, let's proceed to install Bootstrap 5 and integrate it with Angular 14.

Go to your project’s folder:

```bash
$ cd angularbootstrapexample
```


Next, install Bootstrap 5 and jQuery from npm using the following command:

```bash
$ npm install --save bootstrap jquery
```


Next, go the `angular.json` file and add the paths of Bootstrap CSS and JS files as well as jQuery to the `styles` and `scripts` arrays under the `build` target as follows:

```json
"architect": {
  "build": {
    [...], 
    "styles": [
      "src/styles.css", 
        "node_modules/bootstrap/dist/css/bootstrap.min.css"
      ],
      "scripts": [
        "node_modules/jquery/dist/jquery.min.js",
        "node_modules/bootstrap/dist/js/bootstrap.min.js"
      ]
    },
```

## Step 4 -- Creating Angular Components and Setting up Routing


After installing and integrating Bootstrap 4 with your Angular 14 project, let's create some components to test various Bootstrap styles. 

Go to your command-line interface and run the following commands:

```bash
$ ng generate component jumbotron
$ ng generate component bootstrap-form
$ ng generate component bootstrap-table
```

Next, we need to include these components in the routing module to enable multiple views. 

Go to the `src/app/app-routing.module.ts` file and update it as follows:

```ts
import { NgModule } from '@angular/core';
import { Routes, RouterModule } from '@angular/router';
import { BootstrapTableComponent } from './bootstrap-table/bootstrap-table.component';
import { BootstrapFormComponent } from './bootstrap-form/bootstrap-form.component';
import { JumbotronComponent } from './jumbotron/jumbotron.component';

const routes: Routes = [
  {path:  "", pathMatch:  "full",redirectTo:  "home"},
  {path: "jumbotron", component: JumbotronComponent},
  {path: "bootstrap-form", component: BootstrapFormComponent},
  {path: "bootstrap-table", component: BootstrapTableComponent}  
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```

## Step 5 -- Adding A Bootstrap 5 Jumbotron 

[A Bootstrap Jumbotron](https://getbootstrap.com/docs/4.4/components/jumbotron/) is a lightweight, flexible component that can optionally extend the entire viewport to showcase key marketing messages on your site

Let’s add a Bootstrap Jumbotron component to our jumbotron page. 

Head to the `src/app/jumbotron/jumbotron.component.html` file and add the following HTML markup:

```html
<div class="jumbotron" style="height: calc(95vh);">
  <h1>Angular 14 Bootstrap 5 Demo</h1>
  <p class="lead">
    This tutorial teaches you how to integrate Bootstrap 5 with Angular 14  
  </p>
</div>
```

Wu use the built-in `.jumbotron` class to create a Bootstrap Jumbotron.

## Step 6 -- Creating an Angular Bootstrap 5 Table

Let’s now see how to use a Bootstrap 5 table to display tabular data.

Go the `src/app/bootstrap-table/bootstrap-table.component.ts` file and add some data that we can display:

```ts
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-bootstrap-table',
  templateUrl: './bootstrap-table.component.html',
  styleUrls: ['./bootstrap-table.component.css']
})
export class BootstrapTableComponent implements OnInit {

  employees = [
    {id: 1, name: "E 001", description: "E 001 des", email: "e001@email.com"},
    {id: 2, name: "E 002", description: "E 002 des", email: "e002@email.com"},
    {id: 3, name: "E 003", description: "E 003 des", email: "e003@email.com"},
    {id: 4, name: "E 004", description: "E 004 des", email: "e004@email.com"}
  ];
  selectedEmployee;

  constructor() { }

  ngOnInit() {    
  }

  public createEmployee(e: {id, name, description, email}){
    this.employees.push(e);
  }

  public selectEmployee(e){
    this.selectedEmployee = e;
  }
}
```

We simply defined two variables `employees` and `selectedEmployee` for holding the set of employees and the selected employee. And a `selectEmployee()` method which assigns the selected employee to the `selectedEmployee` variable.

Next, go the `src/app/bootstrap-table/bootstrap-table.component.html` file and update it as follows:

```html
<div class="container" style="margin-top: 70px;">
  <table class="table table-hover">
    <thead>
      <tr>
        <th>#</th>
        <th>Name</th>
        <th>Email</th>
        <th>Actions</th>
      </tr>
    </thead>
    <tbody>
      <tr *ngFor="let employee of employees">

        <td>{{ employee.id }}</td>
        <td> {{ employee.name }}</td>
        <td> {{ employee.email }}</td>
        <td>
          <button class="btn btn-primary" (click)="selectEmployee(employee)"> Select</button>
        </td>
      </tr>
    </tbody>
  </table>
  <div class="card text-center" *ngIf="selectedEmployee">
      <div class="card-header">
        # {{selectedEmployee.id}}
      </div>
      <div class="card-block">
        <h4 class="card-title">{{selectedEmployee.name}}</h4>
        <p class="card-text">
          {{selectedEmployee.description}}
        </p>    
      </div>

    </div>
</div>
```


[A Bootstrap 5 Card](https://getbootstrap.com/docs/4.4/components/card/) is a flexible and extensible content container. It includes options for headers and footers, a wide variety of content, contextual background colors, and powerful display options. If you’re familiar with Bootstrap 3, cards replace our old panels, wells, and thumbnails. Similar functionality to those components is available as modifier classes for cards.

We use the built-in `.table` and `.table-hover` classes to create Bootstrap tables, the `.card`, `.card-block`, `.card-title` and `.card-text` classes to create cards. 


## Step 7 -- Adding A Bootstrap 5 Form Component

Let’s proceed by adding a Bootstrap-styled form to the `bootstrap-form` component. 

Next, go to the `src/app/bootstrap-form/bootstrap-form.component.ts` file and update it as follows:

```ts
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'bootstrap-form/-create',
  templateUrl: './bootstrap-form/.component.html',
  styleUrls: ['./bootstrap-form/.component.css']
})
export class BootstrapForm/Component implements OnInit {

  employee : {id, name, description, email} = {id: null, name: "", description: "", email: ""};

  constructor() { }

  ngOnInit() {
  }

  createEmployee(){
    console.log("Employee created: ", this.employee);
    this.employee = {id: null, name: "", description: "", email: ""};

  }
}
```

Next, go to the `src/app/bootstrap-form/bootstrap-form.component.html` file and update it as follows:

```html
<div class="container" style="margin-top: 70px;">

  <div class="row">

    <div class="col-sm-8 offset-sm-2">

      <div>
        <form>
          <div class="form-group">
            <label for="id">ID</label>
            <input [(ngModel)]="employee.id" type="text" name="id" class="form-control" id="id" aria-describedby="idHelp" placeholder="Employee ID">
            <small id="idHelp" class="form-text text-muted">Enter your employee’s ID</small>

            <label for="name">Employee Name</label>
            <input [(ngModel)]="employee.name" type="text" name="name" class="form-control" id="name" aria-describedby="nameHelp" placeholder="Enter your employee name">
            <small id="nameHelp" class="form-text text-muted">Enter your employee’s name</small>

            <label for="email">Employee Email</label>
            <input [(ngModel)]="contact.email" type="text" name="email" class="form-control" id="email" aria-describedby="emailHelp"
              placeholder="Enter your employee email">
            <small id="nameHelp" class="form-text text-muted">Enter your employee’s email</small>

            <label for="description">Employee Description</label>
            <textarea [(ngModel)]="employee.description" name="description" class="form-control" id="description" aria-describedby="descHelp">
                      </textarea>
            <small id="descHelp" class="form-text text-muted">Enter your employee’s description</small>

          </div>
        </form>
        <button class="btn btn-primary" (click)="createEmployee()">Create employee</button>
      </div>
    </div>
  </div>
</div>
```

We make use of the `.form-group` and `.form-control` classes to create a Bootstrap form.

## Step 8 -- Serving your Angular 14 Application

Head over to your command-line interface, and run the following command from the folder of your project:

```bash
$ ng serve
```

A development server will be started at the `http://localhost:4200` address.

After learning how to add Bootstrap to Angular 14, see how to [upload files with Angular 14 and HttpClient](https://efficientcoder.net/angular-14-upload-files-formdata-httpclient/)

## Conclusion

As a recap, we have seen how to initialize an Angular 14 project and integrate it with Bootstrap 4. Next, we used various Bootstrap CSS utilities to create a responsive layout with tables, forms, buttons, cards and jumbotrons.
