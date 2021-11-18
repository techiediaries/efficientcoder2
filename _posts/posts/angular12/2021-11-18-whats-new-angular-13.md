---
layout: post
title:  "What’s new with Angular 13"
---

Another fantastic version of Angular has been published. Here are some of the features from version 13!

The improvements that the Angular team has made and plans to make are leading us to a beautiful place — 2022 should be an exciting time to be an Angular developer.

Here are some of the most significant things from this release. For additional information, see the official Angular blog and roadmap!

## Updates to the Angular Core

### View Engine Support is Being Removed

Angular 13  no longer supports the old rendering View Engine, as stated in previous statements. You may anticipate ngcc to be removed as well (in a future edition). For the average Angular dev, this doesn’t mean much (other than getting the benefits of a more performant framework). This is significant for Angular library builders. So, all of you library authors out there, brace yourself and make sure you’re ready.

### Dynamic Component Creation in the Absence of Component Factories

One Ivy-enabled API update is a cleaner approach to dynamically construct a component. To construct a component, ViewContainerRef.createComponent no longer requires an instantiated factory. Check out Mark’s code comparisons for before and after Ivy’s changes: Updates to the component API.

Support for View Engine backwards compatibility has been withdrawn, as have the following providers for such a use case:

ModuleWithComponentFactories  
Compiler\sCompilerFactory  
JitCompilerFactory  
NgModuleFactory  
More information may be found in the  [CHANGELOG](https://github.com/angular/angular/blob/master/CHANGELOG.md#core-1).

### TestBed Teardown is now the default.

Configurations in TestBed that allow for easier cleanup of test modules and environments are now enabled by default. This functionality was added in v12 and, while it is currently enabled by default, it may be removed or adjusted per module.

### Documentation Enhancements

The documentation were also given greater attention in this version’s release. Check out these $localize documentation to see how they’re introducing additional examples to help with learning and getting started with Angular!

### Adobe Fonts are supported inline.

You may now inline not just your Google Fonts (v11), but also your Adobe Fonts! Check out Mark Thompson’s video on how to use font inling to improve your performance!

### Ending IE11 Support

Angular will no longer support Internet Explorer 11 as of version 13. The team issued an RFC (request for comments) on eliminating IE11 support, and the response was largely positive.

Dropping these polyfills means that everything will be quicker — apps will load faster (because to their reduced size) and implementation will be faster (because of the improved API).

As stated by the team:

> “We’ll be able to remove special JS and CSS code paths, polyfills, build passes, and dev infrastructure that exists only because we support IE11.”

Dropping IE11 is a positive thing since it allows Angular to fully use the Web APIs of contemporary and evergreen browsers. If that is a browser you must support, you will be able to utilize Angular v12 with full support until November 2022. I’m looking forward to this shift and believe that the LTS that Angular v12 is providing is more than generous, given that Microsoft has already discontinued support for IE11 from their Microsoft 365 products.

## Updates to Angular Tooling and Dependencies

### Angular CLI

New Angular projects in v13+ will have persistent build cache enabled by default! This dramatically accelerates the development of Angular apps:

> “A 68% increase in construction speed and additional ergonomic alternatives” — Mike Thompson

This was another subject on which the Angular team solicited feedback and acted on it. The strong amount of support prompted them to add this in v13, so keep an eye out for future RFCs — the team actually listens and cares about the community’s feedback as they drive the framework forward.

### Version 4.4 of TypeScript

TypeScript 4.4 now has complete support as well. Among the many features of TS 4.4 are:

-   Analysis of Control Flow of Aliased Conditions and Discriminants
-   Symbol and Template String Pattern Index Signatures
-   Use the unknown type in Catch Variables by default(–useUnknownInCatchVariables)
-   Exact Optional Property Types (–exactOptionalPropertyTypes)

### Version 7.4 of RxJS

RxJS has now reached version 7. RxJS 7.4 will be the default for new apps generated with the Angular CLI as of Angular v13. If you are still using RxJS 6, you will need to manually perform the following command to update: npm install rxjs@7.4.

## Enhancements to Angular Material Accessibility

Accessibility changes have been made to Angular Material components (which are now based on MDC components).
