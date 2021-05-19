---
layout: post
title:  "Angular 11 Http Interceptors"
---

Angular 11 gives us various built-in utilities to enable scaling out large JavaScript applications. Interceptors are one of the built-in APIs for handling HTTP requests at a global application level.

More often than not, we need to enforce or apply behavior when receiving or sending HTTP requests within our application. Interceptors are a unique type of Angular 11 Service that we can implement. Interceptors allow us to intercept incoming or outgoing HTTP requests using the  `HttpClient`. By intercepting the HTTP request, we can modify or change the value of the request.

In this post, we cover three different Interceptor implementations:

-   Handling HTTP Headers
-   HTTP Response Formatting
-   HTTP Error Handling

This post assumes some basic knowledge of the  [Angular 11 HTTP Client](https://angular.io/guide/http)  and  [RxJS Observables](https://angular.io/guide/rx-library). Let’s take a look at the basic API implementation.

```
import { Injectable } from '@angular/core';
import { HttpInterceptor, HttpEvent, HttpResponse, HttpRequest, HttpHandler } from '@angular/common/http';
import { Observable } from 'rxjs';

@Injectable()
export class MyInterceptor implements HttpInterceptor {
  intercept(httpRequest: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
    return next.handle(httpRequest);
  }
}

```

To create an Interceptor, we need to implement the  `HttpInterceptor`  interface from  `@angular/common/http`  package. Every time our application makes an HTTP request using the  `HttpClient`  service, the Interceptor calls the  `intercept()`  method.

When the  `intercept()`  method is called Angular 11 passes a reference to the  `httpRequest`  object. With this request, we can inspect it and modify it as necessary. Once our logic is complete, we call  `next.handle`  and return the updated request onto the application.

Once our Interceptor is created, we need to register it as a multi-provider since there can be multiple interceptors running within an application. Important note, you must register the provider to the  `app.module`  for it to properly apply to all application HTTP requests. Interceptors will only intercept requests that are made using the  `HttpClient`  service.

```
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { HttpClientModule, HTTP_INTERCEPTORS } from '@angular/common/http';
import { RouterModule, Routes } from '@angular/router';

import { MyInterceptor } from './my.interceptor';
import { AppComponent } from './app.component';

@NgModule({
  imports: [BrowserModule, HttpClientModule],
  declarations: [AppComponent],
  bootstrap: [AppComponent],
  providers: [
    { provide: HTTP_INTERCEPTORS, useClass: MyInterceptor, multi: true }
  ]
})
export class AppModule { }

```

Next, let’s take a look at our first Interceptor implementation by creating an Interceptor that can modify request headers.

## HTTP Header Interceptor

Often we need to return an API key to an authenticated API endpoint via a request Header. Using Interceptors, we can simplify our application code to handle this automatically. Let’s make a simple use case of attaching an API header key to each request.

```
import { Injectable } from '@angular/core';
import { HttpInterceptor, HttpEvent, HttpResponse, HttpRequest, HttpHandler } from '@angular/common/http';
import { Observable } from 'rxjs';
import { map, filter } from 'rxjs/operators';

@Injectable()
export class HeaderInterceptor implements HttpInterceptor {
  intercept(httpRequest: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
    const API_KEY = '123456';
    return next.handle(httpRequest.clone({ setHeaders: { API_KEY } }));
  }
}

```

On the  `httpRequest`  object, we can call the clone method to modify the request object and return a new copy. In this example we are attaching the  `API_KEY`  value as a header to every HTTP request  `httpRequest.clone({ setHeaders: { API_KEY } })`.

Now let’s use the  `HttpClient`  service to make a HTTP get request.

```
import { Component, OnInit } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';
import { tap } from 'rxjs/operators';

@Component({
  selector: 'app-header',
  template: `
    <h2>Header Example</h2>
    <pre>{{ data | json }}</pre>
  `
})
export class HeaderComponent implements OnInit {
  data: {};
  constructor(private httpClient: HttpClient) { }

  ngOnInit() {
    this.httpClient.get('/assets/header.json').subscribe(data => this.data = data);
  }
}

```

If we look at the dev tools in the browser, we can see the network request containing our new header  `API_KEY`  with the corresponding value.



Now with each request, we automatically send our API key without having to duplicate the logic throughout our application.

> Important! For security reasons ensure your Interceptor only sends your API key to the APIs that require it by checking the request URL.

## Formatting JSON Responses

Often we want to modify the request value that we get back from an API. Sometimes we work with APIs that have formatted data that can make it challenging to work within our application. Using Interceptors, we can format the data and clean it up before it gets to our application logic. Let’s take a look at an example API response.

```
{
  "id": "123",
    "metadata": "blah",
    "data": {
      "users": {
      "count": 4,
      "list": [
        "bob",
        "john",
        "doe"
      ]
    }
  }
}

```

In this example, our data we want to render to our component is nested deeply within the response object. The other data is just noise for our app. Using a Interceptor, we can clean up the data.

```
import { Injectable } from '@angular/core';
import { HttpInterceptor, HttpEvent, HttpResponse, HttpRequest, HttpHandler } from '@angular/common/http';
import { Observable } from 'rxjs';
import { map, filter } from 'rxjs/operators';

@Injectable()
export class FormatInterceptor implements HttpInterceptor {
  intercept(httpRequest: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
    return next.handle(httpRequest).pipe(
      filter(event => event instanceof HttpResponse && httpRequest.url.includes('format')),
      map((event: HttpResponse<any>) => event.clone({ body: event.body.data.users.list }))
    );
  }
}

```

With the  `httpRequest`  object, we can inspect the URL of the request and determine if it is a request we want to ignore or modify. If the request is to the  `format`  API endpoint, then we continue and update the response. We also only want to modify the request if the request was a response coming back from our API.

```
filter(event => event instanceof HttpResponse && httpRequest.url.includes('format')),

```

Now that we filter out only the request we care about we can update the response body to be a simple array of the users we want to display.

```
return next.handle(httpRequest).pipe(
  filter(event => event instanceof HttpResponse && httpRequest.url.includes('format')),
  map((event: HttpResponse<any>) => event.clone({ body: event.body.data.users.list }))
);

```

Now in our component, we can subscribe to our data without having to dig into the response object, our API returned.

```
import { Component, OnInit } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';

@Component({
  selector: 'app-format',
  template: `
    <h2>Formated JSON</h2>
    <pre>{{ data | json }}</pre>
  `
})
export class FormatComponent implements OnInit {
  data: {};

  constructor(private httpClient: HttpClient) { }

  ngOnInit() {
    this.httpClient.get('/assets/format.json').subscribe(data => this.data = data);
  }
}

```

## Error Handling

We can leverage Interceptors to also handle HTTP errors. We have a few options on how to handle these HTTP errors. We could log errors through the Interceptors or show UI notifications when something has gone wrong. In this example, however, we will add logic that will retry failed API requests.

```
import { Injectable } from '@angular/core';
import { HttpInterceptor, HttpEvent, HttpResponse, HttpRequest, HttpHandler, HttpErrorResponse } from '@angular/common/http';
import { Observable } from 'rxjs';
import { retry } from 'rxjs/operators';

@Injectable()
export class RetryInterceptor implements HttpInterceptor {
  intercept(httpRequest: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
    return next.handle(httpRequest).pipe(retry(2));
  }
}

```

In our request handler, we can use the RxJS  `retry()`, operator. The  `retry`  operator allows us to retry failed Observable streams that have thrown errors. Angular 11’s HTTP service uses Observables which allow us to re-request our HTTP call. The  `retry`  operator takes a parameter of the number of retries we would like. In our example, we use a parameter of 2, which totals to three attempts, the first attempt plus two additional retries. If none of the requests succeed, then, the Observable throws an error to the subscriber of the HTTP request Observable.

```
import { Component, OnInit } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable, of } from 'rxjs';
import { catchError } from 'rxjs/operators';

@Component({
  selector: 'app-retry',
  template: `<pre>{{ data | json }}</pre>`
})
export class RetryComponent implements OnInit {
  data: {};

  constructor(private httpClient: HttpClient) { }

  ngOnInit() {
    this.httpClient.get('https://example.com/404').pipe(
      catchError(err => of('there was an error')) // return a Observable with a error message to display
    ).subscribe(data => this.data = data);
  }
}

```

In our component, when we make a bad request, we still can catch the error using the  `catchError`  operator. This error handler will only be called after the final attempt in our Interceptor has failed.



Here we can see our three attempts to load the request. HTTP Interceptors are another helpful tool in our toolbox to manage HTTP requests in our Angular 11 applications.

