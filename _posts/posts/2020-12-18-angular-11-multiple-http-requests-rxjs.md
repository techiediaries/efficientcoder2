---
layout: post
title:  "Angular 11 Multiple HTTP Requests with RxJS"
---

More often than not when building SPAs, we need to aggregate data from multiple REST API endpoints and render the fetched data on the UI. 

Making asynchronous requests and working with them can be intimidating but thanks to the Angular 11’s HttpClient service and the RxJS library, it can be implemented more easily. 

We have various ways to make multiple requests; they can be sequential or in parallel. In this tutorial, we will see both by example.

Let’s get started with a simple HTTP request using the Angular 11 HttpClient service:

```typescript
import { Component } from '@angular/core';
import { HttpClient } from '@angular/common/http';

@Component({
  selector: 'app-root',
  templateUrl: 'app/app.component.html'
})
export class AppComponent {
  constructor(private http: HttpClient) {}

  ngOnInit() {
    this.http.get('/api/people/1/').subscribe(json => console.log(json));
  }
}

```

In our example, we have a single component that gets Angular 11’s Http service via Dependency Injection. Angular 11 will provide an instance of the HttpClient service when it sees the signature in our component’s constructor.

Now that we have the service we call the service to fetch some data from our test API. We do this in the  `ngOnInit`. This is a life cycle hook where its ideal to fetch data. You can read more about  `ngOnInit`  in the  [docs](https://angular.io/guide/lifecycle-hooks). For now, let’s focus on the HTTP call. We can see we have  `http.get()`  that makes a GET request to  `/api/people/1`. We then call  `subscribe`  to subscribe to the data when it comes back. When the data comes back, we just log the response to the console. So this is the simplest snippet of code to make a single request. Let’s next look at making two requests.

## Subscribing to HttpClient Observables

In our next example, we will have the following use case: We need to retrieve a character from the  [Star Wars API](https://swapi.dev/). To start, we have the id of the desired character we want to request.

When we get the character back, we then need to fetch that character’s homeworld from the same API but a different REST endpoint. This example is sequential. Make one request then the next"

```typescript
import { Component } from '@angular/core';
import { HttpClient } from '@angular/common/http';

@Component({
  selector: 'app-root',
  templateUrl: 'app/app.component.html'
})
export class AppComponent {
  loadedCharacter: {};
  constructor(private http: HttpClient) {}

  ngOnInit() {
    this.http.get('/api/people/1/').subscribe(character => {
      this.http.get(character.homeworld).subscribe(homeworld => {
        character.homeworld = homeworld;
        this.loadedCharacter = character;
      });
    });
  }
}

```

Looking at the  `ngOnInit`  method, we see our HTTP requests. First, we request to get a user from  `/api/user/1`. Once loaded we the make a second request a fetch the homeworld of that particular character. Once we get the homeworld, we add it to the character object and set the  `loadedCharacter`  property on our component to display it in our template. This works, but there are two things to notice here. First, we are starting to see this nested pyramid structure in nesting our Observables which isn’t very readable. Second, our two requests were sequential. So let’s say our use case is we just want to get the homeworld of our character and to get that data we must load the character and then the homeworld. We can use a particular operator to help condense our code above.

## Using MergeMap

In this example, we will use the  `mergeMap`  operator. Let’s take a look at the code example first.

```typescript
import { Component } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';
import { mergeMap } from 'rxjs/operators';

@Component({
  selector: 'app-root',
  templateUrl: 'app/app.component.html'
})
export class AppComponent {
  homeworld: Observable<{}>;
  constructor(private http: HttpClient) {}

  ngOnInit() {
    this.homeworld = this.http
      .get('/api/people/1')
      .pipe(mergeMap(character => this.http.get(character.homeworld)));
  }
}

```

In this example, we use the  `mergeMap`  also known as  `flatMap`  to map/iterate over the Observable values. So in our example when we get the homeworld, we are getting back an Observable inside our character Observable stream. This creates a nested Observable in an Observable. The  `mergeMap`  operator helps us by subscribing and pulling the value out of the inner Observable and passing it back to the parent stream. This condenses our code quite a bit and removes the need for a nested subscription. This may take a little time to work through, but with practice, it can be a handy tool in our RxJS tool belt. Next, let’s take a look at multiple parallel requests with RxJS.

## Using ForkJoin

In this next example, we are going to use an operator called  `forkJoin`. If you are familiar with Promises, this is very similar to  `Promise.all()`. The  `forkJoin()`  operator allows us to take a list of Observables and execute them in parallel. Once every Observable in the list emits a value, the  `forkJoin`  will emit a single Observable value containing a list of all the resolved values from the Observables in the list. In our example, we want to load a character and a characters homeworld. We already know what the ids are for these resources so we can request them in parallel.

```typescript
import { Component } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable, forkJoin } from 'rxjs';

@Component({
  selector: 'app-root',
  templateUrl: 'app/app.component.html'
})
export class AppComponent {
  loadedCharacter: {};
  constructor(private http: HttpClient) {}

  ngOnInit() {
    let character = this.http.get('https://swapi.dev/api/people/1/');
    let characterHomeworld = this.http.get('http://swapi.dev/api/planets/1/');

    forkJoin([character, characterHomeworld]).subscribe(results => {
      // results[0] is our character
      // results[1] is our character homeworld
      results[0].homeworld = results[1];
      this.loadedCharacter = results[0];
    });
  }
}

```

In our example, we capture the character and characterHomeworld Observable in variables. Observables are lazy, so they won’t execute until someone subscribes. When we pass them into  `forkJoin`  the  `forkJoin`  operator will subscribe and run each Observable, gathering up each value emitted and finally emitting a single array value containing all the completed HTTP requests. This is a typical pattern with JavaScript UI programming. With RxJS this is relatively easy compared to using traditional callbacks.

With the  `mergeMap`/`flatMap`  and  `forkJoin`  operators we can do pretty sophisticated asynchronous code with only a few lines of code. Check out the live example below!
