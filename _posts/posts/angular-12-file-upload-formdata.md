---
layout: post
title:  "Angular 12 File Upload and FormData"
date:   2021-2-24
tags: [angular]
---

Uploading files and images is a common problem in web development and you’ll often encounter a requirement for implementing file uploading in your apps. Depending on your chosen framework or library. For example, Angular, React, or Vue you’ll have different techniques for file uploading, but there are common things – since all these tools are essentially client-side tools that are built on top of JavaScript – such as using FormData structure to create a form in JavaScript or TypeScript and send multi-part data to servers.

While React and Vue.js don’t have their own HTTP client, Angular comes with a built-in HttpClient with advanced features such as progress report and interceptors.

Basically, you would use Angular HttpClient with FormData to send requests to the server for uploading single or multiple files.

You also need to use various artifacts such as Angular services for encapsulating the code for uploading files and Angular components for building the file uploading UI.

For styling the UI. you have a myriad of options such as Angular Material, Bootstrap, and Tailwind, etc. with the first options being the most popular.

In this article, we’ll be learning about the used Angular concepts for file and image upload, including a step-by-step tutorial on how to implement file upload with the latest Angular 12 version, and Bootstrap 4.

Angular is a complete platform for building JavaScript client-side apps also referred to as single-page applications. It’s actually written in TypeScript by a Google team of developers and includes built-in libraries for common web development problems such as sending HTTP requests and handling form submission and validation.

## How File/Image Uploading Works in Angular 12

Angular provides many built-in modules that you can use to make file and image uploads.

Depending on your use case requirements, you may need to modify more or less code but in nutshell, these are the building blocks of any file upload functionality in your apps.

### FormData: Encapsulating File Data

FormData is an HTML5 API that’s built-in in modern web browsers and not specific to Angular. It allows you to create objects for storing key-value pairs that correspond to an HTML form. It can be seen as the JavaScript way of creating forms to send data, including files, to REST API servers via HTTP.

You can create a FormData object using the following code:

```ts
const formData = new FormData();
```

After creating the instance, you can append key-value pairs of data and files that you need to send or upload to the server that exposes a REST API endpoint.

### HttpClientModule: Uploading Files/Images with HTTP Requests

HttpClientModule is a built-in module that contains the HttpClient service for sending HTTP requests and getting responses back from HTTP servers. It is based on the XMLHttpRequest interface available on all web browsers. It provides extra features such as interceptors and typed requests and responses.

## Multiple File/Image Uploading Tutorial

Throughout this tutorial, we’ll be learning how to implement multiple file and image uploading with Angular 12 and previous versions including Angular 11, Angular 10, and Angular 9.

We’ll see step by step how to build an example Angular 12 application with progress bars generated with Angular CLI, styled with Bootstrap 4 for uploading multiple files or images.

Since Angular is a client-side JavaScript framework, we’ll be using FormData for sending file or image data to the API server.

## Implementing Multiple File Upload with Angular 12 and FormData

In this tutorial, you’re going to create an Angular 12 application that allows you to upload multiple files/images upload to a file server. The app will show a progress bar for each of the files that are being uploaded to the server.

## Tools and Libraries

We’ll be using the following tools for building our file uploading demo:

-   Angular CLI 12
-   Bootstrap 4

## REST API Server Endpoint for File Upload

In this part, we’re not going to build the server app for file uploading because this is beyond the scope of Angular. You can use any server-side language for building your REST API servers such as Python with Django or Flask, Node.js with Express or PHP.

We’ll assume that you server will be exposing the following endpoints

- POST /upload Upload a file
- GET /files Get a list of files
- GET /files/[filename] Download a file

## Step 1 – Initializing your Angular 12 Project & Creating the Bare-Bones Artifacts

Let’s get started by initializing your Angular 12 project using Angular CLI, the official tool created by the team behind Angular.

Head over to a new command-line interface and create a new Angular 12 project by running the following command:

```bash
$ ng new Angular12UploadDemo
? Would you like to add Angular routing? No
? Which stylesheet format would you like to use? CSS

```

Next, you need to generate the following artifacts using Angular CLI:

```bash
$ cd Angular12UploadDemo
$ ng g s upload  
$ ng g c upload

```

We generated a component and service for implementing our file upload example. The CLI will generate the artifacts and import them in the  _src/app/app.module.ts_  file.

The Angular service will be available from the  _src/app/upload.service.ts_  file and will need to export two  _upload_  and  _get_  methods for uploading and getting files from the REST API server.

The Angular component will contain the presentational code for laying out our file upload UI, It resides in the  _src/app/upload/upload.component.ts_  file and contains a form and progress bars.

Angular is an opinionated framework with a structure based on NgModules, and components which allow you to build apps with a modular and component-based approach. Your Angular application has a root module and a root component but you can later create as many modules and components you need.

Modules are made up of one or more components. A component controls a part of your application user interface. Components are linked to HTML templates that define the views of your application. Aside from modules and components, we also have other artifacts, such as services that allow you to encapsulate functionality that can be reused throughout your application and take benefit of Angular dependency injection to provide the functionality to any component or even other services, that need it without repeating the code. In this example, we’ll be using a service that encapsulates the code needed for uploading multiple files to a server.

## Step 2 – Importing HttpClientModule

Next, let’s import the HttpClient module in our application.

Open  _src/app/app.module.ts_  file and import  `HttpClientModule`  as follows:

```ts
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';

import { AppComponent } from './app.component';
import { HttpClientModule } from '@angular/common/http';
import { UploadComponent } from './upload/upload.component';

@NgModule({
  declarations: [
    AppComponent,
    UploadComponent
  ],
  imports: [
    BrowserModule,
    HttpClientModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

## Step 3 – Adding Bootstrap 4

After creating the project, you need to install Bootstrap 4 and integrate it with your Angular 12 project.

Head back to your command-line interface and run the following command to install Bootstrap 4 and jQuery from npm:

```bash
$ npm install --save bootstrap jquery
```

(In this case,  **bootstrap v4**  and  **jquery v3**  are installed.)

Finally, open the  `angular.json`  file and add the file paths of Bootstrap CSS and JS files as well as jQuery to the  `styles`  and  `scripts`  arrays under the  `build`  target:

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

## Step 4 – Implementing the Angular 12 Multiple File Upload Service

In the first step, we generated a service for encapsulating the multiple file uploading functionality, in this step we’ll proceed to implement the required methods.

Basically, our service will call the HttpClient methods for sending http requests to the server and will export the following methods:

-   `upload()`: This method will be called by the component to upload a file to the server. It will return  `Observable<HttpEvent<any>>`
-   `getFiles()`: This methods will be called by the component to get a list of files.

Open the src/app/_upload.service.ts_  file and update it as follows:

```ts
import { Injectable } from '@angular/core';
import { HttpClient, HttpRequest, HttpEvent } from '@angular/common/http';
import { Observable } from 'rxjs';

@Injectable({
  providedIn: 'root'
})
export class UploadService {

  private serverUrl = 'http://localhost:8080';

  constructor(private httpClient: HttpClient) { }

  upload(file: File): Observable<HttpEvent<any>> {
    const formData: FormData = new FormData();

    formData.append('file', file);

    const request = new HttpRequest('POST', `${this.serverUrl}/upload`, formData, {
      reportProgress: true,
      responseType: 'json'
    });

    return this.httpClient.request(request);
  }

  getFiles(): Observable<any> {
    return this.httpClient.get(`${this.serverUrl}/files`);
  }
}

```

In the upload method, we use  `FormData`  for storing our form data as key-value pairs and we set  `reportProgress: true`  to instruct the HTTP client to emit progress events.

Finally, we call the  `request()`  method of  `HttpClient`  to send an HTTP POST to upload the files to the Rest server.

In the  _getFiles_  method, we call the get method of HttpClient to send an HTTP Get method for getting the uploaded files from the server.

## Step 5 – Creating our Angular 12 UI for Upload Multiple Files

After implementing the service that encapsulates the code for uploading multiple files or images, let’s now create the UI.

Open the src/app/upload/upload.component.ts file and start by adding the following imports:

```ts
import { Component, OnInit } from '@angular/core';
import { UploadService } from 'src/app/upload.service';
import { HttpEventType, HttpResponse } from '@angular/common/http';
import { Observable } from 'rxjs';
```

Next, define the following variables and inject  `UploadService`  via the component’s constructor as follows:

```ts
export class UploadComponent implements OnInit {

  selectedFiles: FileList;
  progressInfos = [];
  message = '';

  fileInfos: Observable<any>;

  constructor(private uploadService: UploadService) { }
}

```

The  `progressInfos`  array will be used for displaying the upload progress of each file.

Next, add the following code in the  `ngOnInit()`  method to get the files when the component is intialized:

```ts
ngOnInit(): void {
  this.fileInfos = this.uploadService.getFiles();
}

```

Next, define the  `selectFiles()`  method for getting the selected files that you need to upload as follows:

```ts
selectFiles(e): void {
  this.progressInfos = [];
  this.selectedFiles = e.target.files;
}
```

Now, iterate over the selected files above and invoke the  `upload()`  method on each file as follows:

```ts
uploadFiles(): void {
  this.message = '';

  for (let i = 0; i < this.selectedFiles.length; i++) {
    this.upload(i, this.selectedFiles[i]);
  }
}
```

Next, define the  `upload()`  method for uploading each file as follows:

```ts
upload(idx, file): void {
  this.progressInfos[idx] = { value: 0, fileName: file.name };

  this.uploadService.upload(file).subscribe(
    event => {
      if (event.type === HttpEventType.UploadProgress) {
        this.progressInfos[idx].value = Math.round(100 * event.loaded / event.total);
      } else if (event instanceof HttpResponse) {
        this.fileInfos = this.uploadService.getFiles();
      }
    },
    err => {
      this.progressInfos[idx].value = 0;
      this.message = 'Could not upload the file:' + file.name;
    });
}
```

We use  `idx`  for accessing the index of the current file to access the corresponding progress information in the  `progressInfos`  array. Next, we invoke the  `upload()`  method of our service with the  `file`  passed as a parameter.

We calculate the progress percentage using the  `event.loaded`  and  `event.total`  values.

  
Once the file uploading is completed, the event will be a  `HttpResponse`  object. So, we invoke the  `getFiles()`  method of our service to get the files’ information and assign the result to  `fileInfos`  variable.

Next, add the following markup to the  _src/app/upload/upload.component.html_  file to create the UI for uploading multiple files or images with progress bars:

```html
<div *ngFor="let progressInfo of progressInfos" class="mb-2">
  <span>{{ progressInfo.fileName }}</span>
  <div class="progress">
    <div
      class="progress-bar progress-bar-info progress-bar-striped"
      role="progressbar"
      attr.aria-valuenow="{{ progressInfo.value }}"
      aria-valuemin="0"
      aria-valuemax="100"
      [ngStyle]="{ width: progressInfo.value + '%' }"
    >
      {{ progressInfo.value }}%
    </div>
  </div>
</div>

<label class="btn btn-default">
  <input type="file" multiple (change)="selectFiles($event)" />
</label>

<button
  class="btn btn-success"
  [disabled]="!selectedFiles"
  (click)="uploadFiles()">
  Upload
</button>

<div class="alert alert-light" role="alert">{{ message }}</div>

<div class="card">
  <div class="card-header">List of Files</div>
  <ul
    class="list-group list-group-flush"
    *ngFor="let file of fileInfos | async"
  >
    <li class="list-group-item">
      <a href="{{ file.url }}">{{ file.name }}</a>
    </li>
  </ul>
</div>

```

### Adding the Component to App Component

Since we didn’t implement routing in our application and we have only two components – the App and Upload components, we need to include the upload component in the App component using its selector.

Open the src/app/_app.component.html_  file and include the Upload component using the  `<app-upload>`  tag:

```html
<h1>{{ title }}</h1>
 
<div class="container">
  <app-upload></app-upload>
</div>
```

## Step 6 – Running your Angular 12 App

If you have a REST API server running and would like to test your app, you simply need to run the  **_ng serve_**  command from your project’s folder.

Next, navigate to the  `http://localhost:4000/`  address to start playing with your app.

## Conclusion

Throughout this tutorial, we’ve seen by example how to build an example application with Angular 12 for uploading multiple files and images with progress bars using Angular 12 HttpClient and FormData. We styled the UI with Bootstrap 4.
