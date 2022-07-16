---
layout: post
title: "Angular 14 release and features"
tags: [angular, angular-14]
excerpt: "After Angular v13 was made available to the public in November of 2021, the following major update will be Angular 14 and will be released in June 2022 with new features that include standalone components, cli auto-completion and typed reactive forms"
---
Hot news for angular developers! The Angular 14 version will be released soon packed with revolutionary and exiting new features.

After Angular v13 was made available to the public in November of 2021, the following major update will be Angular 14.

As a recap, these are the most important new features of Angular 14:


- Standalone components
- inject()
- Typed forms

## Date of release for the Angular 14 version

The Angular team at Google has announced that they intend to release Angular version 14 in the month of June. i.e., June 2022.

The release candidate 1, also known as Angular version 14.0.0-rc.1, was made available to the public on May 18th, 2022.

## Update to Angular CLI v4

The Angular 14 version has not yet been made available to the public.

However, we can still install Angular version 14 with the `next` flag.

Simply, open a new command line interface and run the following command:

```bash
$ npm install -g @angular/cli@next
```

You can verify the installed version using the following command:

```bash
ng v 
```

You should see a similar output:

```bash
     _                      _                 ____ _     ___
    / \   _ __   __ _ _   _| | __ _ _ __     / ___| |   |_ _|
   / △ \ | '_ \ / _` | | | | |/ _` | '__|   | |   | |    | |
  / ___ \| | | | (_| | |_| | | (_| | |      | |___| |___ | |
 /_/   \_\_| |_|\__, |\__,_|_|\__,_|_|       \____|_____|___|
                |___/


Angular CLI: 14.0.0-rc.1
Node: 16.13.1
Package Manager: npm 8.1.2
OS: win32 x64

Angular:
...

Package                      Version
------------------------------------------------------
@angular-devkit/architect    0.1400.0-rc.1 (cli-only)
@angular-devkit/core         14.0.0-rc.1 (cli-only)
@angular-devkit/schematics   14.0.0-rc.1 (cli-only)
@schematics/angular          14.0.0-rc.1 (cli-only)
```

## Principal features of Angular 14

Angular 14 has a plethora of a new features that can make angular developers lives much easier especially when it comes to smplify how we write and design apps and reduce the boilerplate code and the complexity of the framework.

## Angular 14 standalone components

A world without angular modules or NgModules!

As stated, Angular 14 brings some amazing new features such as standalone components (also directives and pipes) that make ngmodules optional. This means that we can now architecture our apps in diffrent ways without using modules for each feature. 

This also brings things closer to how we design apps with popular user interface libraries such as React.

The use of NgModules may be avoided with the help of standalone components, directives, and pipes. This will result in an improved authoring experience.. The new standalone design may be adopted in a gradual and optional manner by already existing apps without causing any breaking changes.

There is a preview version of the standalone component functionality available for developers so there is a possibility that it may evolve before being stable.

Here is an example of a standalone component:

```ts
@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  standalone: true,
  styleUrls: ['./app.component.scss']
})
export class AppComponent {}
bootstrapApplication(AppComponent, {}).catch(err => console.error(err));
```

We simply set the `standalone` property to true to make the component standalone.

## The `inject()` function

We may retrieve a reference for a token from the injector that is currently active by using the `inject()` function that Angular provides. However, it could only be called in services and factory providers.

With Angular 14, we can call the inject function inside components, directives, and pipes. This paves the way for a whole new set of opportunities and possibilities. We are able to design functions that can be reused by using the Angular's dependendcy injection.

Angular 14's inject function will fundamentally transform the way in which we make use of services. There are a lot of different things we could do with it.

Here is an example of how we can turn `HttpClient.get` into a method that may be reused in other contexts. There is no longer a requirement for a constructor for a basic get request:

```ts
import { inject } from '@angular/core';
import { HttpClient } from '@angular/http/client';

function get(url: string) {
  return inject(HttpClient).get(url);
}

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  standalone: true,
  styleUrls: ['./app.component.scss']
})
export class AppComponent{
  data$ = get('YOUR_URL');
}
```

You can also read more about [angular 14 features](https://angulargraphqlbook.com/angular-14-release-date-features/) here.

## Conclusion

Building Angular apps is made easier starting with version 14, thanks to the introduction of standalone components. 

