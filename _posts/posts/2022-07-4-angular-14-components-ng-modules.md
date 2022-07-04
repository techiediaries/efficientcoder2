---
layout: post
title:  "Angular 14 Components and NgModules"
---

In this tutorial, We'll learn about Angular 14 components and modules, then walk you through adding some components for the expense tracker app we will create together.  
  
Angular 14 is a framework for creating front-end applications using HTML, CSS and JavaScript. It is one of the top JavaScript frameworks for building dynamic web applications. In a  [previous tutorial](https://www.efficientcoder.com/angular-14-tutorial-environment-project-setup), I talked about some Angular 14 command-line interface basics, set up an Angular 14 project, and looked at some of the configurations for an Angular 14 project.

In this tutorial, We'll learn about Angular 14 components and modules, then walk you through adding some components for the expense tracker app we will build together. If you skipped the previous tutorial, you can download the source code on  [GitHub](https://github.com/pmbanugo/expense-tracker-angular)  and copy the files from  `src-part-1`  into the  `src`  folder, in order to follow along.

## What Is a Component?

Angular 14 apps are built on a component-based architecture. This means that the app is divided into independent components, where each component renders a specific set of elements on the page and can be combined to display a functional and interactive UI to the users.

An Angular 14 component determines what gets displayed on the screen. They should be designed in such a way that the page is segmented, with each section having a backing component. This means that a page/view can have components arranged in a hierarchy, allowing you to show and hide entire UI sections based on the application’s logic. In other words, you can nest components inside another component, having something like a parent-child relationship.

An Angular 14 component is made up of:

1.  Template: A template is a set of HTML elements that tells Angular 14 how to display the component on the page.
2.  Style: A list of CSS style definitions that applies to the HTML elements in the template.
3.  Class: A class that contains logic to control some of what the template renders, through an API of properties and methods.

### The Angular 14 Root Component

An Angular 14 example app must have at least one component, which is the root component and under which other components are nested. The generated example app already has a root component bootstrapped for you. That’s why if you run  `ng serve`  to run the app, you see elements rendered to the screen. You’ll find the component in  `src/app/`  folder.

You should notice the three constituents of a component, which we talked about. The  **app.component.css**  contains the style,  **app.component.html**  contains the template, and  **app.component.ts**  is the class for the component. Having  `.component.`  as part of the file name doesn’t make it a component. It’s a naming convention adopted by the Angular 14 community, which makes it easy to identify what type of file it is.

Open  **app.component.html**  to see the content of that file. You should see HTML elements you should be familiar with. But, you should also notice  `{{ title }}`  on line 4, which is how you would bind data from the component’s class, and also`<router-outlet></router-outlet>`  on line 21, which is a directive used when you’re working with the Angular 14 router module. We will cover those in a future tutorial.

Open the  **app.component.ts**  file. It should have the code below in it:

```javascript
import { Component } from "@angular/core";

@Component({
  selector: "et-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"]
})
export class AppComponent {
  title = "expense-tracker-angular";
}

```

This TypeScript file defines and exports a class. The class is adorned with  `@Component`  decorator. You may be familiar with decorators in JavaScript (which are still in the proposal stage). It’s the same concept in TypeScript. They provide a way to add annotations to class declarations and members. The class decorator is applied to the constructor of the class and can be used to observe, modify, or replace a class definition. It is this decorator that makes the class a component’s class.

The decorator receives metadata, which tells Angular 14 where to get the other pieces it needs to create the component and display its view. This is how it associates the template and style with the class. The  `templateUrl`  option specifies where to find the template for this component. The  `styleUrls`  option also specifies the location of the file for the styles. The  `selector`  option is how the component will be identified in the template’s HTML. For example, if Angular 14 finds  `<et-root></et-root>`  in HTML within the app, it’ll insert an instance of this component between those tags. You’ll notice the  `<et-root></et-root>`  tag in  **src/index.html**.

The associated class has one property  `title`, with the value  `expense-tracker-angular`. The class properties contains data that can be referenced in the template. Remember the  `{{ title }}`  snippet in the template? Angular 14 will replace that with the data in that property.

## NgModules and the Angular 14 Root Module

Angular 14 apps are designed to be modular, and this is achieved through a modularity system called  **NgModules**. NgModules (or Angular 14 modules) is a technique used to create a loosely coupled but highly cohesive system in Angular 14. A module is a collection of components, services, directives and pipes (I will talk more about pipes and directives later). We use this to group a set of functionality in the app, and can export or import other modules as needed.

Angular 14 module is one of the fundamental building blocks in Angular 14. Thus, every Angular 14 application must have at least one module — the root module. This root NgModule is what’s used to bootstrap the Angular 14 application. It is in this root module that we also bootstrap the root-level component. This root-level component is the application’s main view, which hosts other components for the application.

You will find the root NgModule for the expense tracker app you’re createing in  `src/app/app.module.ts`. The content of the file looks like the following:

```javascript
import { BrowserModule } from "@angular/platform-browser";
import { NgModule } from "@angular/core";

import { AppRoutingModule } from "./app-routing.module";
import { AppComponent } from "./app.component";

@NgModule({
  declarations: [AppComponent],
  imports: [BrowserModule, AppRoutingModule],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule {}

```

An NgModule is a class adorned with the  `@NgModule`  decorator. The  `@NgModule`  takes a metadata object that describes how to compile the module. The properties you see are described below:

1.  **declarations**: Declares which components, directives and pipes belong to the module. At the moment, just the root  `AppComponent`.
    
2.  **imports**: Imports other modules with their components, directives, and pipes that components in the current module need. You should notice the  [BrowserModule](https://angular.io/api/platform-browser/BrowserModule)  being imported. This module exports the  [CommonModule](https://angular.io/api/common/CommonModule)  and  [ApplicationModule](https://angular.io/api/core/ApplicationModule)  — NgModules needed by Angular 14 web apps. They include things like the  _NgIf_  directive, which you’ll use in the next tutorial, as well as core dependencies that are needed to bootstrap components.
    
3.  **bootstrap**: Specifies the main application root component, which hosts all other app views/components, and is needed when bootstrapping the module. This root component is inserted into  **src/index.html**. Only the root NgModule should set the bootstrap property in your Angular 14 app.
    

The bootstrapping process creates the components listed in the bootstrap array and inserts each one into the browser DOM. Each bootstrapped component is the base of its own tree/hierarchy of components. Inserting a bootstrapped component usually triggers a cascade of component creations that fill out that component-tree. Many applications have only one component tree and bootstrap a single root component.

The root module is bootstrapped by calling  `platformBrowserDynamic().bootstrapModule(AppModule)`  in  **src/main.ts**

## Adding Bootstrap

Now that we have covered Angular 14 module and component basics, and have seen how they’re constructed by looking at the root component and root module, we’re going to add bootstrap and change the current layout of the app. To install bootstrap, run:

```shell
npm install bootstrap

```

Shell

This adds bootstrap as a dependency to the project. Next import the style in the global style file. Open  **src/styles.css**  and paste the code below in it.

```css
@import "~bootstrap/dist/css/bootstrap.min.css";

```

This adds bootstrap to the global styles for the application.

## Creating Components

We will add a component that will hold a summary of the current and previous months’ total expenses. We will use the Angular 14 command-line interface to generate the component. Open the command line and run  `ng generate component expenses/briefing-cards`  command. This generates the files needed for the  `briefing-cards`  component and adds that component to the declaration in the root module. If you check  **app.module.ts**, you should see the component gets imported and added to the module’s declaration.

You’re going to update the component's HTML template as you see below. Open  **src/app/expenses/briefing-cards/briefing-cards.component.html**  and update it.

```html
<div class="row">
  <div class="col-sm-3">
    <div class="card">
      <div class="card-header">
        August
      </div>
      <div class="card-body">
        <div style="font-size: 30px">$300</div>
      </div>
    </div>
  </div>
  <div class="col-sm-3">
    <div class="card">
      <div class="card-header">
        September
      </div>
      <div class="card-body">
        <div style="font-size: 30px">$90</div>
      </div>
    </div>
  </div>
</div>

```

In this template, we hardcoded values. We will make this component dynamic in the next tutorial where I will cover data binding. The component class is in  **briefing-cards.component.ts**. It already is decorated with @Component and the necessary files referenced. The selector is prefixed with the selector prefix configured for the project.

Next, we’ll add another component called  `expense-list`. Open the command line and run the command  `ng g c expenses/expense-list`. This creates the needed files for the component. We still used the  `ng generate`  command, except that this time we used alias  `g`  for generate and  `c`  for the component argument. You can read more about this command in the  [documentation](https://angular.io/cli/generate).

Open  **expense-list.component.html**  and paste the markup below in it.

```html
<table class="table">
  <caption>
    <button type="button" class="btn btn-dark">Add Expense</button>
  </caption>
  <thead class="thead-dark">
    <tr>
      <th scope="col">Description</th>
      <th scope="col">Date</th>
      <th scope="col">Amount</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Laundry</td>
      <td>12-08-2019</td>
      <td>$2</td>
    </tr>
    <tr>
      <td>Dinner with Shazam</td>
      <td>21-08-2019</td>
      <td>$2500</td>
    </tr>
  </tbody>
</table>

```

The template is already wired up with the component class, and the component added to the declaration in the root module since we used the  `ng generate`  command. This is where Angular 14 command-line interface helps with productivity. Coding styles that adhere to loosely coupled and cohesive design are used by the command-line interface and necessary file changes are made for you.

## Nested Components

Components are designed to have a single responsibility — a piece of the page they should control. How you put this together is by using a component inside another component, thereby creating a hierarchy of components/view, which will all add up to display the necessary layout on the screen.

For the expense tracker app, we want to have the home page display a navigation header, and then the two views from the two components you created below it.

Run the command below to generate a new component.

```shell
ng g c home

```

Go to the component’s HTML template file and add the following:

```html
<et-briefing-cards></et-briefing-cards>
<br />
<et-expense-list></et-expense-list>

```

This way, we’re using those components in the  `Home`  component, by referencing them using the selector identifier specified in the  `@Component`  decorator for those components. When the app runs, Angular 14 will render the component’s view where it finds the respective component’s directive in the template.

Open the template for the root app component (i.e src/app/app.component.html) and update it with the following HTML template:

```html
<nav class="navbar navbar-expand-lg navbar-dark bg-dark">
  <a class="navbar-brand" href="#">Expense Tracker</a>
  <button
    class="navbar-toggler"
    type="button"
    data-toggle="collapse"
    data-target="#navbarNavAltMarkup"
    aria-controls="navbarNavAltMarkup"
    aria-expanded="false"
    aria-label="Toggle navigation"
  >
    <span class="navbar-toggler-icon"></span>
  </button>
  <div class="collapse navbar-collapse" id="navbarNavAltMarkup">
    <div class="navbar-nav">
      <a class="nav-item nav-link active"
        >Home <span class="sr-only">(current)</span></a
      >
      <a class="nav-item nav-link">History</a>
    </div>
  </div>
</nav>

<div class="container">
  <br />
  <et-home></et-home>
</div>
```

The new markup for the root component’s view contains code to display a navigation header and then the  `Home`  component. You can test the application to see how the new things you added render in the browser. Open your command-line application and run  `ng serve -o`. This starts the development server, and opens the application in your default browser.

![angular-app (002)](https://d585tldpucybw.cloudfront.net/sfimages/default-source/default-album/angular-app-(002).png?sfvrsn=f08bbe79_0 "angular-app (002)")

## Wrap-up

In this tutorial, you learned about Angular 14 components and modules. Components are a way to define the various views in an application. With this, you can segment the page into various partitions and have individual components deal with an area of the page. You learned about the constituent parts of an Angular component, what the  `@Component`  decorator does, and how to include a component to a module so that it’s accessible for every component that needs it. You also learned about Angular modules, which is a way to organize an application and extend it with capabilities from external libraries. Angular modules provide a compilation context for their components. The root NgModule always has a root component that is created during bootstrap.

We went through the default root module and component generated by the command-line interface, and I showed you how to create components to define the view of your application. We used static text, but in the next tutorial, We'll learn about data binding and more, so we can start to make the app dynamic, which is the main purpose of using Angular by the way.
