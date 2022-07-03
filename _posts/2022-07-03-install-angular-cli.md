---
layout: post
title:  "How to Install Angular 14 CLI and Create an Angular 14 Project with Routing"
date:   2022-07-03
---

[Angular 14 id released with a lot of new features](https://efficientcoder.net/angular-14-release-features/). Regarding the CLI, the most important new feature is auto completion for commands that will save you the time for looking for the right command or options.


In this tutorial, we'll install the latest Angular CLI 14 version and generate a new Angular 14 project with routing.

Let's get started.

## Step 1 — Setting up Angular CLI 14

In our first step, we'll proceed to install the latest Angular CLI 14 version.

> **Note**: You can also use previous versions of Angular with this tutorial.


Open a new command-line interface and run the following command:

```bash
$ npm install -g @angular/cli
```

This will install **angular/cli v14.0.0** at the time of writing this tutorial.

In the next step, we'll proceed to create a new example project from the command-line.

## Step 2 — Creating a New Angular 14 Project

In our second step, we'll  use Angular CLI to create our example project. Go back to your terminal and run the following commands:

```bash
$ cd ~
$ ng new angular-example-with-routing

```

You'll get prompted for a couple of things — If  **Would you like to add Angular routing?**  Say  **y** and  **Which stylesheet format would you like to use?**  Pick  **CSS**.

This will set up routing in our project and set CSS as the stylesheets format for components.

Next, go to the root folder of your project and run the local development server using the following commands:

```bash
$ cd angular-example-with-routing
$ ng serve    

```

A live-reload server will be running from  the `http://localhost:4200/`  address:


