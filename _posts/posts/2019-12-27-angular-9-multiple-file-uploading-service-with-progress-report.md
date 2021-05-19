---
layout: post
title:  "Angular 9/8 Multiple File Uploading Service with Progress Report"
date:   2019-12-27
---



In this how-to tutorial, we'll see how to build a multiple file/image uploading service with Angular 9 which allows you to upload files to a remote server via a POST request method to listen for progress events.


## Prerequisites

We'll only see how to implement the service for sending a POST request to upload files and listen for progress events but we'll not create a project from scratch so you'll need to install Angular CLI and initialize a project.

## Step 1 - Creating an Angular Service

Open a new terminal and run the folowing command to generate a service:    

```bash
$ ng generate service upload
```

Next, open the  `src/app/upload.service.ts`  file and import the following:

```
import { HttpClient, HttpEvent, HttpErrorResponse, HttpEventType } from  '@angular/common/http';  
import { map } from  'rxjs/operators';

```

Next, inject  `HttpClient`  via the service constructor:

```
@Injectable({  
  providedIn: 'root'  
})  
export class UploadService { 
	constructor(private httpClient: HttpClient) { }

```

## Step 2 - Implementing the Upload Method
 
Next, add the  `upload()`  method which simply calls the post() method of HttpClient to send an HTTP POST request with form data to the file upload server:

```
public upload(fileData) {

	return this.httpClient.post<any>("https://file.io/", fileData, {  
      reportProgress: true,  
      observe: 'events'  
    });  
}
```

## Conclusion

In this quick how-to article, we've implemented a service for uploading files to a server and listenning for progress events.
   
 
