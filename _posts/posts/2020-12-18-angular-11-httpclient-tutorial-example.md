---
layout: post
title:  "Angular 11 HttpClient: Tutorial and Example"
---



Throughout this Angular 11 tutorial, we'll learn step by step how to consume a REST API using Angular HttpClient. 

So, we will cover in details consuming the REST API using Angular 11 HttpClient and other required modules:

- Imporing Angular 11 HttpClient
- Angular 11 HttpClient Error Handling
- Angular 11 HTTPHeaders
- Angular 11 HttpClient POST, PUT, and DELETE Request
- Angular 11 HttpParams Example


## Getting Started

Let's get started in this tutorial with generating an Angular 11 app using Angular CLI. First, we will set up Angular CLI in our development machine using the following command:

```bash
sudo npm install -g @angular/cli
```

Next, create a new Angular 11 app using Angular CLI by type this command.

```
ng new angular-httpclient-example
```

That command will create a new Angular 11 app with the name `angular-httpclient` and pass all questions as default then the Angular CLI will automatically install the required NPM modules. After finished, go to the newly created Angular 11 folder then run the Angular 11 app for the first time.

```
cd ./angular-httpclient-example
ng serve --open
```

Using that "--open" parameters, will automatically open the Angular 11 in your default web browser. Here's the Angular 11 default page look like.

## Creating a Local JSON File or Remote REST API Service

For this tutorial, we will use one of the Local JSON file or the remote REST API service that will consume or call by Angular 11 HttpClient. For local JSON file, simply, create a new folder inside `src/assets`.

```
mkdir src/assets/data
touch src/assets/data/smartphone.json
```

Open and edit `src/assets/data/smartphone.json` then add these lines of JSON data.

```
[
  {
    "id": "P0001",
    "name": "iPhone X",
    "desc": "iPhone X 128GB Internal Memory, 8GB RAM, 6 Inch IPS Display",
    "price": 499,
    "updated": "Wed Sep 20 2019 17:22:41 GMT+0700"
  },
  {
    "id": "P0002",
    "name": "iPhone 11 Pro",
    "desc": "Super Retina XDR display, 5.8‑inch (diagonal) all‑screen OLED Multi‑Touch display, HDR display, 2436‑by‑1125-pixel resolution at 458 ppi 2,000,000:1 contrast ratio (typical)",
    "price": 699,
    "updated": "Wed Sep 20 2019 17:22:41 GMT+0700"
  },
  {
    "id": "P0003",
    "name": "iPhone XR",
    "desc": "Liquid Retina HD display 6.1-inch (diagonal) all-screen LCD Multi-Touch display with IPS technology 1792-by-828-pixel resolution at 326 ppi 1400:1 contrast ratio (typical)",
    "price": 599,
    "updated": "Wed Sep 20 2019 17:22:41 GMT+0700"
  }
]
```

Now, the local JSON file is ready to use with your Angular 11 HttpClient. Next, to prepare the remote REST API, we already have the source codes and tutorials for you with MongoDB or PostgreSQL.



You can choose one of it and make sure you have installed the required database (PostgreSQL or MongoDB) then you can clone that Node-Express REST API server and run from your local machine.

## Imporing Angular 11 HttpClient

The Angular 11 HttpClient simplified the client HTTP API on the XMLHttpRequest interface exposed by browsers. The Angular HttpClient has features of testability features, typed request and response objects, request and response interception, Observable APIs, and streamlined error handling. This module already included when creating a new Angular app. We just need to register it this Angular app. Open and edit `src/app/app.module.ts` then add this import of HttpClientModule that is part of @angular/common/http.

```
import { HttpClientModule } from '@angular/common/http';
```

Add it to @NgModule imports array after the BrowserModule.

```
imports: [
  BrowserModule,
  HttpClientModule
],
```

Now, the Angular 11 HttpClient is ready to use or inject with the Angular 11 service or component.

##  Basic Local JSON or RESP API Request Example

We will put all REST API or JSON requests in the Angular Service. To do that, generate an Angular Service using this Angular Schematic command.

```
ng g service api
```

Open and edit `src/app/api.service.ts` then add this import of HttpClient that part of @angular/common/http.

```
import { HttpClient } from '@angular/common/http';
```

Inject that HttpClient module to the constructor of the Angular service.

```
constructor(private http: HttpClient) { }
```

Declare a constant variable before the @Injectable that hold the local URL of JSON file.

```
const localUrl = 'assets/data/smartphone.json';
```

You change to the URL of your REST API server if you are using remote REST API. Add a function below the constructor to get the data from the JSON file.

```
getSmartphone() {
  return this.http.get(localUrl);
}
```

Now, you can access the JSON call from the Angular component. Open and edit `src/app/app.component.html` then add or modify these imports of Angular OnInit and ApiService.

```
import { Component, OnInit } from '@angular/core';
import { ApiService } from './api.service';
```

Inject the ApiService to the constructor.

```
constructor(private api: ApiService) {}
```

Add a variable before the constructor that holds the response from the data subscription.

```
smartphone: any = [];
```

Add a new function to subscribe to data that emits from the ApiService then put it to an array variable that previously declared with the specific fields.

```
getSmartphones() {
  this.api.getSmartphone()
    .subscribe(data => {
      for (const d of (data as any)) {
        this.smartphone.push({
          name: d.name,
          price: d.price
        });
      }
      console.log(this.smartphone);
    });
}
```

If you run the app now, you can see the array of data in the browser console that returned from the ApiService like this.

```
(3) [{…}, {…}, {…}]
0: {name: "iPhone X", price: 499}
1: {name: "iPhone 11 Pro", price: 699}
2: {name: "iPhone XR", price: 599}
length: 3
```

The previous function uses a blind (any) type for the response of the subscribed data and returns the only array of data as shown in the JSON file. Now, we will show you to use a type and full response which including special headers or status codes to indicate certain conditions that are important to the application workflow. We can create a new file to declare those fields of type or directly create an export interface of the type in the same component file. For this example, we will use a separate Typescript file.

```
touch src/app/smartphone.ts
```

Next, open and edit `src/app/smartphone.ts` then add these lines of type fields as the exported interface.

```
export interface Smartphone {
  id: string;
  name: string;
  desc: string;
  price: number;
  updated: Date;
}
```

Next, back to the service then add or modify these imports.

```
import { HttpClient, HttpResponse } from '@angular/common/http';
import { Smartphone } from './smartphone';
import { Observable } from 'rxjs';
```

Change the GET data function to this function.

```
getSmartphone(): Observable<HttpResponse<Smartphone[]>> {
  return this.http.get<Smartphone[]>(
    localUrl, { observe: 'response' });
}
```

Next, back to the component then add this import of the above type interface.

```
import { Smartphone } from './smartphone';
```

Change the variable type from any to Smartphone type.

```
smartphone: Smartphone[] = [];
```

Modify the function that calls the ApiService.

```
getSmartphones() {
  this.api.getSmartphone()
  .subscribe(resp => {
    console.log(resp);
    const keys = resp.headers.keys();
    this.headers = keys.map(key =>
      `${key}: ${resp.headers.get(key)}`);

    for (const data of resp.body) {
      this.smartphone.push(data);
    }
    console.log(this.smartphone);
  });
}
```

Now, you will see the full response.

```
HttpResponse {headers: HttpHeaders, status: 200, statusText: "OK", url: "http://localhost:4200/assets/data/smartphone.json", ok: true, …}body: (3) [{…}, {…}, {…}]headers: HttpHeaders {normalizedNames: Map(7), lazyUpdate: null, lazyInit: null, headers: Map(7)}ok: truestatus: 200statusText: "OK"type: 4url: "http://localhost:4200/assets/data/smartphone.json"__proto__: HttpResponseBase
```

To GET data by ID simply add this function to the API service.

```
getSmartphoneById(id: any): Observable<any> {
  return this.http.get<Smartphone>(localUrl + id).pipe(
    retry(3), catchError(this.handleError<Smartphone>('getSmartphone')));
}
```

In the component add this function.

```
getSmartphoneById(id: any) {
  this.api.getSmartphoneById(id)
    .subscribe(data => {
      console.log(data);
    });
}
```

## Angular 11 HttpClient Error Handling

We have to add an error handling to handle any error on every HttpClient requests. To do that, simply open and edit `src/app/api.service.ts` then add this function that handles the error in a simple way.

```
private handleError<T>(operation = 'operation', result?: T) {
  return (error: any): Observable<T> => {
    console.error(error);
    this.log(`${operation} failed: ${error.message}`);

    return of(result as T);
  };
}

private log(message: string) {
  console.log(message);
}
```

In the above codes, we are creating 2 functions for handle error and print log of error. There are the new required modules, so, add it to existing imports.

```
import { Observable, of } from 'rxjs';
```

Next, add a HttpClient request function with catchError.

```
getSmartphone(): Observable<any> {
  return this.http.get<Smartphone[]>(localUrl).pipe(
    catchError(this.handleError<Smartphone[]>('getSmartphone', [])));
}
```

Change the get data function in the component to this function.

```
getSmartphones() {
  this.api.getSmartphone()
    .subscribe(data => {
      console.log(data);
    });
}
```

Another way to handle the error is by using HttpInterceptor. To do that, create a new folder `src/app/interceptors` and an HttpErrorInterceptor file.

```
mkdir src/app/interceptors
touch src/app/interceptors/HttpErrorInterceptor.ts
```

Next, open and edit `src/app/interceptors/HttpErrorInterceptor.ts` then add these imports of RxJS Observable, throwError, catchError, tap, Angular Common Http HttpInterceptor, HttpRequest, HttpHandler, HttpEvent, and HttpErrorResponse.

```
import { catchError, tap } from 'rxjs/internal/operators';
import { Injectable } from '@angular/core';
import { HttpInterceptor, HttpRequest, HttpHandler, HttpEvent, HttpErrorResponse } from '@angular/common/http';
import { Observable, throwError } from 'rxjs';
```

Add these Typescript class that intercepts error from the HttpRequest.

```
@Injectable()
export class HttpErrorInterceptor implements HttpInterceptor {
  intercept(request: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
    return next.handle(request)
      .pipe(
        tap(data => console.log(data)),
        catchError((error: HttpErrorResponse) => {
          if (error.error instanceof ErrorEvent) {
            // A client-side or network error occurred. Handle it accordingly.
            console.error('An error occurred:', error.error.message);
          } else {
            // The backend returned an unsuccessful response code.
            // The response body may contain clues as to what went wrong,
            console.error(
              `Backend returned code ${error.status}, ` +
              `body was: ${error.error}`);
          }
          // return an observable with a user-facing error message
          return throwError(
            'Something bad happened; please try again later.');
        })
      );
  }
}
```

Next, open and edit `src/app/app.module.ts` then add this import of Angular Common Http HttpInterceptor and HttpErrorInterceptor that previously created.

```
import { HTTP_INTERCEPTORS } from '@angular/common/http';
import { HttpErrorInterceptor } from './interceptors/HttpErrorInterceptor';
```

Add them to the @NgModule provider's array.

```
providers: [
  {
    provide: HTTP_INTERCEPTORS,
    useClass: HttpErrorInterceptor,
    multi: true,
  }
],
```

You can use Retry function to retry the HttpRequest after an error is occurred to make sure the HTTP request is a real error. Add this function after the pipe function inside HttpErrorInterceptor or service function.

```
getSmartphone(): Observable<any> {
  return this.http.get<Smartphone[]>(localUrl).pipe(
    retry(3), catchError(this.handleError<Smartphone[]>('getSmartphone', [])));
}
```

Don't forget to modify existing RxJS import.

```
import {catchError, retry} from 'rxjs/internal/operators';
```

## Angular 11 HTTPHeaders

Almost in every HTTP requests including headers. Sometimes, REST API servers required additional headers parameters on every request. For example, the secured REST API endpoint only accessible with an Authorization header token, the specific REST API request use a different type of response by determining the type from the HTTP headers. To add the header to this HttpClient example, in the ApiService file add or modify this import of @angular/common/http HttpHeaders.

```
import { HttpHeaders } from '@angular/common/http';
```

Add a constant variable before the @Injectable that determine the HttpHeaders parameters.

```
const httpOptions = {
  headers: new HttpHeaders({
    'Content-Type':  'application/xml',
    'Authorization': 'jwt-token'
  })
};
```

Add that httpOptions to the required HTTP request. In this example, modify the existing GET request.

```
getSmartphone(): Observable<any> {
  return this.http.get<Smartphone[]>(localUrl, httpOptions).pipe(
    retry(3), catchError(this.handleError<Smartphone[]>('getSmartphone', [])));
}
```

The HTTP request headers will look like this.

```
GET /assets/data/smartphone.json HTTP/1.1
Host: localhost:4200
Connection: keep-alive
Accept: application/json, text/plain, */*
Authorization: jwt-token
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/77.0.3865.90 Safari/537.36
Sec-Fetch-Mode: cors
Content-Type: application/json
Sec-Fetch-Site: same-origin
Referer: http://localhost:4200/
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9,id;q=0.8
Cookie: _ga=GA1.1.257623774.1563284797; __atuvc=63%7C35%2C22%7C36%2C7%7C37%2C7%7C38%2C4%7C39
If-None-Match: W/"341-9uZs5GFl2K0WHOI2X11xYKnJlvM"
```

Because the instance of HttpHeaders class is immutable, we can modify HttpHeaders values directly.

```
getSmartphone(): Observable<any> {
  httpOptions.headers = httpOptions.headers.set('Authorization', 'my-new-auth-token');
  return this.http.get<Smartphone[]>(localUrl, httpOptions).pipe(
    retry(3), catchError(this.handleError<Smartphone[]>('getSmartphone', [])));
}
```

And here's the headers change.

```
...
Authorization: my-new-auth-token
...
```

## Angular 11 HttpClient POST, PUT, and DELETE Request

To send data to the REST API is really simple with Angular HttpClient. Still, in the same API service file, add a function to POST data to the REST API.

```
addSmartphone(smartphone: Smartphone): Observable<Smartphone> {
  return this.http.post<Smartphone>(localUrl, smartphone, httpOptions)
    .pipe(
      catchError(this.handleError('addSmartphone', smartphone))
    );
}
```

Back to the Angular component, declare the data variable with a type and a variable for the response.

```
spresp: any;
postdata: Smartphone;
```

Add this function to perform POST data using ApiService.

```
addSmartphone() {
  this.api
    .addSmartphone(this.postdata)
    .subscribe(resp => {
      return this.spresp.push(resp);
    });
}
```

Where postdata variable is a typed object that populates from the Angular Form. To send data to the REST API for updating using the PUT method, add a function to PUT data to the REST API.

```
updateSmartphone(id: any, smartphone: Smartphone): Observable<Smartphone> {
  return this.http.put<Smartphone>(localUrl + id, smartphone, httpOptions)
    .pipe(
      catchError(this.handleError('addSmartphone', smartphone))
    );
}
```

Back to the Angular component, declare the data variable with a type and a variable for the response.

```
spresp: any;
postdata: Smartphone;
```

Add this function to perform POST data using ApiService.

```
updateSmartphone(id: any) {
 this.api
   .updateSmartphone(id, this.postdata)
   .subscribe(resp => {
     return this.spresp.push(resp);
   });
}
```

Where the ID variable is an ID of an object and postdata variable is a typed object that populates from the Angular Form after GET from the ApiService. To send data to REST API for deleting using the DELETE method, add a function to DELETE data to the REST API.

```
deleteSmartphone(id: any, smartphone: Smartphone): Observable<Smartphone> {
  return this.http.delete<Smartphone>(localUrl + id, httpOptions)
    .pipe(
      catchError(this.handleError('addSmartphone', smartphone))
    );
}
```

Add this function to perform DELETE data using ApiService.

```
deleteSmartphone(id: any) {
  this.api
    .deleteSmartphone(id)
    .subscribe(resp => {
      return this.spresp.push(resp);
    });
}
```

Where this function just required an ID of the Data that will be deleted.


## Angular 11 HttpParams Example

We can add custom URL parameters using Angular HttpParams. Still, on the ApiService file, add this function of the HttpParams usage example.

```
getSmartphone(term: string): Observable<any> {
  term = term.trim();
  const options = term ? { params: new HttpParams().set('name', term) } : {};
  httpOptions.headers = httpOptions.headers.set('Authorization', 'my-new-auth-token');
  return this.http.get<Smartphone[]>(localUrl, options).pipe(
    retry(3), catchError(this.handleError<Smartphone[]>('getSmartphone', [])));
}
```

Don't forget to add or modify the import of HttpParams.

```
import { HttpClient, HttpResponse, HttpErrorResponse, HttpParams } from '@angular/common/http';
```

Also, the HttpParams can be extracted from the string. Add this function to use get HttpParams from the string.

```
getSmartphone(): Observable<any> {
  const stringparams = 'name=iphone';
  const params = new HttpParams({fromString: stringparams});
  const options = { params };
  return this.http.get<Smartphone[]>(localUrl, options).pipe(
    retry(3), catchError(this.handleError<Smartphone[]>('getSmartphone', [])));
}
```

## Conclusion

We have covered Angular 11 HttpClient by example. It's a common development problem to fetch and consume a REST API server using Angular 11 HttpClient. 

We have covered the following points:

- Imporing Angular 11 HttpClient
- Angular 11 HttpClient Error Handling
- Angular 11 HTTPHeaders
- Angular 11 HttpClient POST, PUT, and DELETE Request
- Angular 11 HttpParams Example


