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

## Conclusion

Building Angular apps is made easier starting with version 14, thanks to the introduction of standalone components. 

