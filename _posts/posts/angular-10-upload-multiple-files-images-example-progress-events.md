---
layout: post
title: "Angular 10 Upload Multiple Files/Images By Example with Progress Events"
image: "images/content/angular.png"
excerpt: "In this article, we’ll be learning about the used Angular concepts for file and image upload, including a step-by-step tutorial on how to implement file upload with the latest Angular 10 version, and Bootstrap 4"
date: 2020-09-14
tags : [angular]
---

Uploading files and images is a common problem in web development and you’ll often encounter a requirement for implementing file uploading in your apps. Depending on your chosen framework or library. For example, Angular, React, or Vue you’ll have different techniques for file uploading, but there are common things – since all these tools are essentially client-side tools that are built on top of JavaScript – such as using FormData structure to create a form in JavaScript or TypeScript and send multi-part data to servers.

While React and Vue.js don’t have their own HTTP client, Angular comes with a built-in HttpClient with advanced features such as progress report and interceptors.

Basically, you would use Angular HttpClient with FormData to send requests to the server for uploading single or multiple files.

You also need to use various artifacts such as Angular services for encapsulating the code for uploading files and Angular components for building the file uploading UI.

For styling the UI. you have a myriad of options such as Angular Material, Bootstrap, and Tailwind, etc. with the first options being the most popular.

In this article, we’ll be learning about the used Angular concepts for file and image upload, including a step-by-step tutorial on how to implement file upload with the latest Angular 10 version, and Bootstrap 4.

Angular is a complete platform for building JavaScript client-side apps also referred to as single-page applications. It’s actually written in TypeScript by a Google team of developers and includes built-in libraries for common web development problems such as sending HTTP requests and handling form submission and validation.

## How File/Image Uploading Works in Angular

Angular provides many built-in modules that you can use to make file and image uploads.

Depending on your use case requirements, you may need to modify more or less code but in nutshell, these are the building blocks of any file upload functionality in your apps.

### FormData: Encapsulating File Data

FormData is an HTML5 API that’s built-in in modern web browsers and not specific to Angular. It allows you to create objects for storing key-value pairs that correspond to an HTML form. It can be seen as the JavaScript way of creating forms to send data, including files, to REST API servers via HTTP.

You can create a FormData object using the following code:

const formData = new FormData();

After creating the instance, you can append key-value pairs of data and files that you need to send or upload to the server that exposes a REST API endpoint.

### HttpClientModule: Uploading Files/Images with HTTP Requests

HttpClientModule is a built-in module that contains the HttpClient service for sending HTTP requests and getting responses back from HTTP servers. It is based on the XMLHttpRequest interface available on all web browsers. It provides extra features such as interceptors and typed requests and responses.

## Multiple File/Image Uploading Tutorial

Throughout this tutorial, we’ll be learning how to implement multiple file and image uploading with Angular 10 and previous versions including Angular 9, Angular 8, and Angular 7.

We’ll see step by step how to build an example Angular 10 application with progress bars generated with Angular CLI, styled with Bootstrap 4 for uploading multiple files or images.

Since Angular is a client-side JavaScript framework, we’ll be using FormData for sending file or image data to the API server.

## Implementing Multiple File Upload with Angular 10 and FormData

In this tutorial, you’re going to create an Angular 10 application that allows you to upload multiple files/images upload to a file server. The app will show a progress bar for each of the files that are being uploaded to the server.

## Tools and Libraries

We’ll be using the following tools for building our file uploading demo:

-   Angular CLI 10
-   Bootstrap 4

## REST API Server Endpoint for File Upload

In this part, we’re not going to build the server app for file uploading because this is beyond the scope of Angular. You can use any server-side language for building your REST API servers such as Python with Django or Flask, Node.js with Express or PHP.

Read the full step-by-step tutorial of how to [upload files and images with Angular 10](https://shabang.dev/angular-10-upload-multiple-files-images-example-progress-events/).


