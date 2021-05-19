---
layout: post
title:  "Angular 11 Data Binding and NgIf and NgFor Directives"
---

Understand how to apply data binding in Angular 11 and how to work with the NgFor and NgIf Directives.

Angular 11 is a framework for building dynamic command-line interfaceent-side applications using HTML, CSS, and JavaScript. It is one of the top JavaScript frameworks for building dynamic web applications. In this tutorial, I'll teach you data binding, using structural directives, and how to pass data from one component to another.

This tutorial builds on two other tutorials. There I teached you how to  [set up an Angular 11 app](https://www.telerik.com/blogs/a-practical-guide-to-angular-environment-and-project-setup), and how to [create modules, create components and group app features into modules](https://www.efficientcoder.net/angular-11-components-ng-modules). You can skip reading those tutorials if you're familiar with setting up an Angular 11 application using the command-line interface, and what components and modules are and how to create and use them.

If you want to code along, you can download the source code on  [GitHub](https://github.com/pmbanugo/expense-tracker-angular). Copy the content of  `src-part-2`  folder to  `src`  folder and follow the instructions I'll give you as you read.

## Data Binding

Data binding is a technique to pass data from the component's class to the view. It creates a connection between the template and a property in the class such that when the value of that property changes, the template is updated with the new value. Currently, the  `briefing-cards`  component displays static numbers. We want to make this dynamic and allow the value to be set from the component's class. Open its component's class and the following properties to it.

```javascript
@Input() currentMonthSpending: object;
@Input() lastMonthSpending: object;

```

JavaScript

Add import for the  `@Input`  decorator on line one:

```javascript
import { Component, OnInit, Input } from "@angular/core";

```

JavaScript

You just added two new properties with type set to  `object`  because we don't want to create a new type for the data. The  `@Input()`  decorator is a way to allow a parent component to pass data to a child component. We want the data for those properties to come from the parent component which is  `home`. With that in place, we now want to bind this property to the template. Update the  `briefing-cards`  component template with the code below:

```html
<div class="row">
  <div class="col-sm-3">
    <div class="card">
      <div class="card-header">
        {{ lastMonthSpending.month }}
      </div>
      <div class="card-body">
        <div style="font-size: 30px">${{ lastMonthSpending.amount }}</div>
      </div>
    </div>
  </div>
  <div class="col-sm-3">
    <div class="card">
      <div class="card-header">
        {{ currentMonthSpending.month }}
      </div>
      <div class="card-body">
        <div style="font-size: 30px">${{ currentMonthSpending.amount }}</div>
      </div>
    </div>
  </div>
</div>

```

HTML

It is almost the same code as before, except now we use a template syntax  `{{ }}`  in lines 5, 8, 15, and 18. This is referred to as interpolation and is a way to put expressions into marked-up text. You specify what you want it to resolve in between the curly braces, and then Angular 11 evaluates it and converts the result to a string which is then placed in the markup.

## Using the NgIf and NgFor Directives

We want to also replace the static data in  `expense-list`  to use data from the component's logic. Open  **expense-list.component.ts**, and add a reference to the  _@Input_  decorator:

```javascript
import { Component, OnInit, Input } from "@angular/core";

```

JavaScript

Add the following property to the component's class:

```javascript
@Input() expenses: IExpense[] = [];
@Input() showButton: boolean = true;

```

JavaScript

The  `showButton`  property is mapped to a boolean type, with a default value that gets assigned to it when the class is initialized. The  `expenses`  property will hold the data to be displayed in the table element. It is bound to a type of  `IExpense`. This type represents the expense data for the application. The property will be an array of  `IExpense`, with the default value set to an empty array.

Go ahead and create the type by adding a new file  **src/app/expenses/expense.ts**. Add the code below in it.

```javascript
export default interface IExpense {
  description: string;
  amount: number;
  date: string;
}

```

JavaScript

We defined an interface type called  `IExpense`, with properties to hold the expense data. An interface defines a set of properties and methods used to identify a type. A class can choose to inherit an interface and provide the implementation for its members. The interface can be used as a data type and can be used to define contracts in the code. The  `IExpense`  type that's set as the type for the  `expenses`  property would enforce that the value coming from the parent component matches that type, and it can only contain an array of that type.

Open  **expense-list.component.ts**  and add an import statement for the newly defined type.

```javascript
import IExpense from "../expense";

```

JavaScript

With our component's logic set to support the template, update  **expense-list.component.ts**  with the markup below:

```html
<table class="table">
  <caption *ngIf="showButton">
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
    <tr *ngFor="let expense of expenses">
      <td>{{ expense.description }}</td>
      <td>{{ expense.date }}</td>
      <td>${{ expense.amount }}</td>
    </tr>
  </tbody>
</table>

```

HTML

You updated the template to make use of data binding and also used some directives. On line 2, you should notice  `*ngIf="showButton"`  and on line 13 you should see  `*ngFor="let expense of expenses"`. The  `*ngIf`  and  `*ngFor`  are known as structural directives. Structural directives are used to shape the view by adding or removing elements from the DOM. An asterisk  `(*)`  precedes the directive's attribute name to indicate it's a structural directive.

The  **NgIf**  directive (denoted as  `*ngIf`) conditionally adds or removes elements from the DOM. It's placed on the element it should manipulate. In our case, the  `<caption>`  tag. If the value for  `showButton`  resolves to true, it'll render that element and its children to the DOM.

The  **NgFor**  directive (used as  `*ngFor`) is used to repeat elements it is bound to. You specify a block of HTML that defines how a single item should be displayed and then Angular uses it as a template for rendering each item in the array. In our example, it is the  `<tr />`  element with the columns bound to the data of each item in the array.

## Passing Data to Child Components

The  `home`  component is the parent to  `briefing-cards`  and  `expense-list`  components. We're going to pass the data they need from the parent down to those components. This is why we defined the data properties with  `@Input`  decorators. Passing data to another component is done through property binding.

Property binding is used to set properties of target elements or component's  _@Input()_  decorators. The value flows from a component's property into the target element property, and you can't use it to read or pull values out of target elements.

Let's go ahead and apply it. Open  **src/app/home/home.component.ts**. Add the properties below to the class definition:

```javascript
expenses: IExpense[] = [
  {
    description: "First shopping for the month",
    amount: 20,
    date: "2019-08-12"
  },
  {
    description: "Bicycle for Amy",
    amount: 10,
    date: "2019-08-08"
  },
  {
    description: "First shopping for the month",
    amount: 14,
    date: "2019-08-21"
  }
];
currentMonthSpending = { amount: 300, month: "July" };
lastMonthSpending = { amount: 44, month: "August" };

```

JavaScript

Then add the import statement for the  `IExpense`  type.

```javascript
import IExpense from "../expenses/expense";

```

JavaScript

Open  **home.component.html**  and add property binding to the component directives as you see below:

```html
<et-briefing-cards
  [lastMonthSpending]="lastMonthSpending"
  [currentMonthSpending]="currentMonthSpending"
></et-briefing-cards>
<br />
<et-expense-list [expenses]="expenses"></et-expense-list>

```

HTML

The enclosing square brackets identify the target properties, which is the same as the name of the properties defined in those components.

With that set up, let's test that our code is working as expected. Open the command line and run  `ng serve -o`  to start the application. This launches your default browser and opens the web app.

## Wrap-up

In this tutorial, you undertood how to use the NgIf and NgFor structural directives. I also showed you how to apply data binding to make the app dynamic and use  _@Input_  decorators to share data between components.
