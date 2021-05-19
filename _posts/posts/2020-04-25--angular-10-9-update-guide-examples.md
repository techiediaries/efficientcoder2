---
layout: post
title:  "Angular 10/9 Update Guide with Examples"
date:   2020-04-25
tags: [angular]
---


In this guide, we'll learn how to upgrade our project to the latest Angular 10 version and update the dependencies.

We have two scenerios:

- You have an Angular 8 project and want to update to Angular 10
- You have an Angular 9 project and want to update to Angular 10

## Updating your Angular 8 Project to Angular 10

Before we can start upgrading your project to Angular 10 from v8, you first need to update it to the latest patch version of Angular 9.

You also need to make sure you do the following changes in your code when appropriate:

- Swap `NgForm`  selector.
- Change `@ContentChild`  and  `@ContentChildren`  hosts.
- Remove assigned values to template-only variables.
- Remove the `Renderer`  directive and replace it with `Renderer2`.

Remember this for your Angular 8 project before updating to Angular 9.


### Update `ngForm` to `ng-form`

If you are using Angular forms in your templates with the `<ngForm>` directive, you need to update any instance of `<ngForm>`  with `<ng-form>` instead. 


For example, if you have declared a form like below:

```html
<ngForm #exampleForm="ngForm">
    <input [(ngModel)]="userName" name="userName" />
</ngForm>
```

You'll simply need to update it as follows:

```html
<ng-form #exampleForm="ngForm">
    <input [(ngModel)]="userName" name="userName" />
</ng-form>
```

### Update `@ContentChild`  and  `@ContentChildren`  Hosts

The  `@ContentChild`  and  `@ContentChildren`  decorator that are used to query the DOM will no longer be able to match their directive's own host node.

Before:

```typescript
@Directive({
  selector: '[myActions]'
})
export class MyDirective implements AfterContentInit {
  // [TODO]: Angular v9 ContentChild will not return host element!!
  @ContentChild(MyDirective, { static: true, read: ElementRef })
  selfElementRef: ElementRef;

  constructor(private readonly renderer: Renderer2) {}

  ngAfterContentInit() {
    const el = this.selfElementRef.nativeElement as HTMLElement;
    if (!el) {
      return;
    }
    this.renderer.setStyle(el, 'color', 'black');
  }
}
```

We used the  `@ContentChild`  content query to access the host  `ElementRef`  for the directive. 

You need to change the previous code to access `ElementRef`  via an injected dependency in the directive as follows:

```typescript
@Directive({
  selector: '[myActions]'
})
export class MyDirective implements AfterContentInit {
  constructor(
    private readonly elementRef: ElementRef,
    private readonly renderer: Renderer2
  ) {}

  ngAfterContentInit() {
    const el = this.elementRef.nativeElement as HTMLElement;
    if (!el) {
      return;
    }
    this.renderer.setStyle(el, 'color', 'black');

  }
}
```

This way, we don't need to use the  `@ContentChild()`  and  `@ContentChildren()`  decorators to query a host element. 


### Do Not Assign Values to Template-only Variables

Thanks to the Ivy renderer which is used by default in Angular 9+, templates have type checking.

Before the Angular 9 version, template-only variables had the TypeScript  `any`  type i.e you can change the object and use unknown properties as you need and the compiler will not complain.

Starting with Angular version 9 which uses the Ivy renderer by default, template-only variables are strongly typed based on your TypeScript compiler options for strictness of template type checking. So you need to avoid changing template-only variables in directly your templates.

Let's look at an example that  _does_  mutate template-only variables:

```html
<button #myBtn (click)="myBtn.state = 'clicked'">
  Click Me
</button>
```

The  `myBtn`  template-only variable has the `any` type in the Angular 8 version. 

Starting With the Angular 9 version, the template variable becomes strongly typed with the built-in  `HTMLButtonElement` type. 

Therefore we'll have an error since the  `HTMLButtonElement`  interface doesn't have a  `state`  member.

To fix this, you need to refactor any changes of template-only variables, and replace them with the component methods. For exmple:

```html
<button (click)="onClick()">
  Click Me
</button>
```

We simply removed the template-only variable and added the `onClick()` method and the `state` property in the component's class:

```typescript
export class MyComponent {
  state = 'not clicked';

  onClick(): void {
    this.state = 'clicked';
  }
}
```


### Replace `Renderer`  with `Renderer2`

The `Renderer` directive  is deprecated on Angular 8 and removed  in Angular 9. 

You need to replace it with the `Renderer2` directive instead.

Fortunately for us, this can automatically done with the  `ng update`  command. 

## Updating your Angular 9 Project to Angular 10

Start from this section, if you already have an Angular 9 project and want to upgrade it to Angular 10.

## Update to the Latest Angular 9 Patch

Before updating your project to the latest Angular 10 version, make sure to start by updating it to the latest stable release of Angular 9.

Head back to your terminal and run the following command:

```bash
$ ng update @angular/core@9 @angular/cli@9
```

Make sure you add the version number for Angular 9 to install the latest patch of this version otherwise, Angular 10 will be installed.

## After Upgrading to Angular 9

If you have updated your project to the Angular 9 version, you only have a couple of things to do:

- You need to migrate your code to use the `TestBed.inject<T>()`  method instead of the `TestBed.get()`  method.
- You need to remove the  `entryComponents`  properties in the  `@NgModule()`  decorator of your modules since they are no longer required.



## Update to the Angular 10 Version

If you have added the required changes to your code and updated your Angular project to the latest Angular 9 patch, you are ready to update it to Angular 10  using the `ng update` command.

Head back to your terminal and run the following command:

```bash
$ ng update @angular/cli @angular/core
```

>Note: Since Angular 10 is not the final release at the current time, you'll need to use the  `--next`  flag as follows:

```bash
$ ng update @angular/cli @angular/core --next
```



## Conclusion

In this guide, we have seen what it takes to update an Angular 9/8 project to the new Angular 10 version with examples.