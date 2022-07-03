---
layout: post
title:  "Angular ngFor with Index and trackBy Example"
date:   2022-07-04
---

In this article, we'll see by examples how to use the `ngFor` directive to iterate over arrays of data and even objects in Angular templates.

You'll read about the following subjects:

- Introducing Angular ngFor
- How to Use Angular ngFor
- The Index of ngFor Elements
- The ngFor trackBy 

## Introducing Angular ngFor

Angular makes use of HTML for templates associated with components which eventually represent the views of your front-end application. 

A component controls a part of your application user interface and the associated template is what gets rendered in that part of the UI.

Since HTML doesn't have a built-in template language, Angular extends HTML with a powerful template syntax that includes many directives such as `ngFor` which is similar to the typical for-loops in programming languages.

The `ngFor` allows you to loop through an array of data directly in the HTML template. The array must be present in the associated component.

## How to Use Angular ngFor

Let's now see how to use the ngFor directive by example.

Let's suppose. we have a movies component and we want to display the movies from an array in the template. Here is how:

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-movies-list',
  template: `
    <ul>
      <li *ngFor="let movie of moviesArr">
        {{ movie.title }}
      </li>
    </ul>
    `
})

export class MoviesListComponent  {
    moviesArr: any[] = [
    {
      "title": "Super Man"
    },
    {
      "title": "Spider Man"
    },
    {
      "title": "Aladdin"
    }, 
    {
      "title": "Downton Abbey"
    }
  ];
}
```

The `moviesArr` array is automatically available in the component's template which is in our case an inline template.

We use the ngFor directive to iterate over the array of movies and display the title of each movie.

The directive has the `*ngFor="let movie of moviesArr"` syntax. After the `let` keyword, we add a variable, which can be any valid variable name, that will be used to reference and access each element of the array and after the `of` keyword, we add the array of data which must be present in the component.

We also need to prefix `ngFor` with a `*` before providing the iteration expression. 

## The Index of ngFor Elements

Suppose that we need to display the index of each element of the movies array. 

Angular provides the reserved `index` keyword inside the `ngFor` expression which can be used as follows:



```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-movies-list',
  template: `
    <ul>
      <li *ngFor="let movie of moviesArr; let i = index">
         {{i}} - {{ movie.title }}
      </li>
    </ul>
    `
})

export class MoviesListComponent  { /* ... */ }
```

Inside the `ngFor` expression, we defined another variable called `i` which gets assigned the `index` keyword which simply contains the current index of each element in each iteration. And we used interpolation to display the value of the `i` and `movie.title` variables. 

> Note: The index starts from 0 not 1.

Angular also provides the reserved `first` and `last` keywords for getting the first and last items in the array.

## The ngFor trackBy 

Angular provides the `trackBy` feature which allows you to track elements when they are added or removed from the array for performance reasons.

we simmpl need to define a `trackBy` method, which needs to identify each element uniquely, in the associated component and assign it to the `ngFor` directive as follows:

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-movies-list',
  template: `
    <ul>
      <li *ngFor="let movie of moviesArr; trackBy: trackByMethod">
        {{ movie.title }}
      </li>
    </ul>
    `
})

export class MoviesListComponent  {
    moviesArr: any[] = [
    {
      "id": 1,
      "title": "Super Man"
    },
    {
      "id": 2,
      "title": "Spider Man"
    },
    {
      "id": 3,
      "title": "Aladdin"
    }, 
    {
      "id": 4,
      "title": "Downton Abbey"
    }
  ];
   
   trackByMethod(index:number, el:any): number {
    return el.id;
  }
}
```

We can simply identify each element in a unique way using its `id`.


## Conclusion

We can use the Angular ngFor directive to iterate over arrays of data right in the Angular's component template.

We can use other features like `index`, `first`, `last` and `trackBy` to get the index of the current element, the first and last elements and for tracking the addition or removal of the array elements for performane reasons.  







