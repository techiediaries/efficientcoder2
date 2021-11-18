---
layout: post
title:  "An Angular 13 Practical Tutorial"
---

Angular 13 is a front-end web framework that allows you to build apps with HTML, CSS, and TypeScript. In this article, the first in a series, I explore how to set up an Angular 13 app using the command-line interface, the different command-line interface commands and parameters, and what most of the command-line interface artifacts achieve.

Angular 13 includes functionality for online, mobile web, native mobile, and native desktop development. It is comparable to well-known JavaScript frameworks like React and Vue. Some of its key features are as follows:

- Native mobile and desktop development with technologies such as Ionic and NativeScript. 
- Encourages the organization of logic into modules, making it easier to manage and reuse logic.
- Because to the tooling assistance, developer productivity has increased.

Throughout the tutorial series, you will master the essential ideas of Angular 13 while developing a tiny real-world sample project.

## Create an Angular 13 sample project

To create an Angular 13 project, we'll use the Angular 13 CLI.

The Angular CLI is a command-line interface program that is used to produce Angular projects, artifacts like components and services, configure and run tests, serve and build production apps, and eventually assist with deployment.

You must first install Node and npm on your development workstation before you can use Angular CLI. You may either download and install it directly from the official download website or use an utility like NVM to do so.

### Angular CLI Installation

You must launch a new terminal and enter the following command:

```bash
npm install -g @angular/cli
```

After installing the CLI, execute the `ng` command, which should display a list of possible command-line interface commands along with their explanations.

We'll begin by using the command `ng new` to create a new Angular 13 example project. By typing `ng new --help`, you may get a list of the various options for this command.

Navigate to the working folder in which you wish to build your Angular 13 project and execute the following command:

```
ng new angular-app -v=true —skipTests=true —skipGit=true —style=css —routing=true
```

This will generate a new Angular 13 example app based on the options you choose. The following is an explanation of those options:

- -v=true: The -v option is an abbreviation for —verbose. It specifies whether you want the command-line interface to output extra information to the console when beginning, producing the required artifacts, and installing the required packages.
- –skipTests=true: This tells the example app not to include test files when you produce files using the command-line interface. This choice was chosen since I will not be discussing how to test Angular 13 sample apps in this course.
- –skipGit=true: When set to true, the project's git repository is not created.
- –routing=true: If set to true, it will construct a routing module for the sample app. You'll find out how this works later.
- –style=css: Defines the file extension or preprocessor for style files.
- –prefix=et: Defines the prefix that will be applied to the project's generated selectors. Selectors are the names you provide to Angular 13 components, and they are utilized when they are rendered as HTML elements on the page. More of this will be shown when we look at Angular 13 components in the upcoming tutorial.

## Anatomy of the Angular 13 Project

In the last section, we created an Angular 13 project using the `ng new` command. This program makes a new app and establishes an Angular 13 workspace directory. A workspace can include many applications, with the first app generated being at the workspace's root level. The source files for the root-level example app are located in the same directory as the workspace.

The root-level example app has the same name as the workspace, and the source files are located in the workspace's src subfolder. The sample app in our instance is called expense-tracker-angular.

The workspace root directory contains the source files for the example applications as well as the workspace and example apps' configuration files. The `tslint.json` file contains the workspace's default TSLint settings. TSLint is a static analysis tool for TypeScript code that tests it for readability, maintainability, and functionality issues.

The `tsconfig.json` file contains the TypeScript settings for the workspace's projects. The configuration file for the karma test runner is `karma.conf.js`.

The `.editorconfig` file contains code editor settings.

The `angular.json` file contains workspace-wide and project-specific configuration defaults for the Angular 13 command-line interface's build and development tools. The top-level `e2e/` directory contains source files for end-to-end tests that correspond to the root-level app, as well as test-specific configuration files. The browserlist file specifies how target web browsers and Node.js versions are shared among various front-end technologies. More information may be found on the GitHub website.

Examine some of the configuration in the `angular.json` file. The following is a list of some of the properties found in that file:

- The value defaultProject is set to the root-level app name **angular-app**. For commands when the project name is not supplied as part of the parameters, this value will be utilized as the project name.

- newProjectRoot: Defines the path where new projects are generated. The workspace directory might be absolute or relative.

- projects: This section contains a sub-section for each application in the workspace, as well as the per-project setup options. We just have one project in the workspace, which you can view by hovering over it. The project also has its own set of setup options, which are discussed further below.
- **projectType**: This determines whether the project is an application or a library. A library cannot start on its own in a web browser, although an application can.
- schematics: A collection of schematics that change the project's ng create sub-command option settings. Angular 13 generation schematics are guidelines for changing a project by adding or updating files. You'll see that "skipTests" is true for some of the schematics. This is in reference to the —skipTests=true option that we used when we performed the command ng new. This command instructs the command-line interface not to create test files for those files when they are generated.

- **root**: This provides the root directory for the files in this project, relative to the workspace directory. It is empty for the root-level app, which is located at the workspace's top level.

sourceRoot: The directory containing the source files for this project. It's src, the root-level application, for the project we're working on.

prefix: This is the name that Angular 13 appends to created component selectors. Remember the —prefix=et flag we specified for the ng new command?

More information on the angular.json configuration file can be found in the docs.

Moving on to the example app's files in the src directory, you should notice the style.css file, which contains the CSS definitions for the example app. It allows you to add/import styles that will be applied globally. It's possible you you noticed it in the styles definition in angular.json.

The primary HTML page delivered when the example program is accessed in a web browser is located in the src/index.html file. Because the command-line interface automatically includes all of the JavaScript and CSS you describe when building the app, you usually don't need to explicitly add any `<script>` or `<link>` tags here. Instead of manually putting them here, declare them in the `angular.json` config and they will be injected automatically.

The `src/environments` directory contains build configuration options for several target environments.

The `src/assets/` directory contains images and other assets.

The sample app's entry point is main.ts. It bootstraps the sample app's root module (AppModule) to start in the web browser using Angular 13's JIT compiler. 

The `app/app.module.ts` file defines this root module. Angular 13 utilizes this module to bundle your example app with the logic and data you provide for the project. 

You also have the app's root component declared with an `et-root` selector in the `app/` directory, which is what is utilized to display the root application view in `index.html`.

You'll note the custom directive `<et-root></et-root>` in the body of `index.html`, which is used to display what's rendered to the screen.

In this article, I will not go over modules and components. In coming lessons, I'll go through those ideas as we develop our application.

## Serving the Angular 13 application

You created an Angular 13 project using the command-line interface. It creates the root module and components required to launch a front-end web app. To build and run the Angular 13 app, open a terminal window, navigate to the directory containing your Angular 13 workspace, and type the `ng serve -o` command. This builds the application and launches a live reload development server to provide the application files.

The `ng serve` command is used to build and serve the Angular 13 application. This, like the other commands you've used so far, includes a handful of options. When the program is finished being built, the `-o` option you just used will launch it in a web browser. There are a variety of additional possibilities available to you. More information may be found in the documentation.

## Wrap-up

We've covered several important Angular 13 concepts. You understand why you'll need the command-line interface, how to set it up, and how to use it to create a new Angular 13 app. You went through the most of the various files created by the command-line interface and what they perform. I demonstrated various options used with the `ng new` and `ng serve` commands. You must also grasp the various configuration files created for the project, as well as what specific `angular.json` options signify.

We didn't include anything pertaining to the example application that we want to create. We'll get started in the following tutorial, where I'll discuss about Angular 13 components.
