---
layout: post
title:  "Angular 11 CSS Classes with NgClass"
---

In this tutorial you’ll see by examples how to use the NgClass directive in Angular 11 to dynamically add class names to DOM elements or Angular 11 components.

The NgClass directive syntax is concise and supports more complex logic, to allow us to have finer control over our class names.

First, let’s revise the HTML and JavaScript way and then see how to add or remove classes with Angular 11’s many built-in methods.

## CSS Class Names Overview

In HTML, we can write declare a class via the  `class`  attribute:

```
<div 
  class="list">
</div>

```

If we wanted to add a class to it, we could use the  `className`  property that exists on  [Element objects](https://developer.mozilla.org/en-US/docs/Web/API/Element/className)  to set or get a class:

```
const el = document.querySelector('.list');
el.className += ' active';
console.log(el.className); // 'list active'

```

## Angular 11 Property Binding with className

First, let’s investigate the  `className`  property binding with Angular 11. This approach allows us to set a class via the native  `className`  property that we demonstrated above.

To bind to the  `className`  property using Angular 11, we’ll need to using the  `[]`  binding syntax to directly bind to a property:

```
<div 
  [className]="'active'">
</div>

```

Angular 11 binds to the  `className`  property and passes our string value  `'active'`  as data.

We can now compose more complex  `[className]`  logic using  [Angular 11 expressions](https://angular.io/guide/template-syntax#expression-context)  with the ternary operator:

```
<div 
  [className]="isActive ? 'active' : 'inactive'">
</div>

```

If  `isActive`  results to  `true`  our  `'active'`  class is added, otherwise  `'inactive'`  would remain, giving us some toggling capability. This is just one example, and it’s not common that we would  _need_  to supply  `'inactive'`  in this case.

You could use  `className`  to compose more complex classes, but this defeats the purpose of using NgClass in Angular 11. However, before we do let’s look at how to use Angular 11’s binding syntax to toggle a class.

## Angular 11 Property Binding with “class”

To toggle a class with Angular 11, we can use the  `[class.class-name]`  syntax to supply a condition to be evaluated:

```
<div 
  class="list" 
  [class.active]="isActive">
</div>

```

> 📢 This  `[class]`  syntax is actually part of  [Angular 11’s NgClass](https://angular.io/api/common/NgClass)  directive, through  `@Input('class')`.

When our  `isActive`  results to  `true`  the  `active`  class will be added (otherwise removed). Also note that we can combine  `class="list"`  with  `[class.active]`  (this does  _not_  work with  `[className]`  they cannot be combined).

This  `[class]`  syntax also supports  [kebab-case](https://medium.com/better-programming/string-case-styles-camel-pascal-snake-and-kebab-case-981407998841#a084)  (using a hyphen between words such as  `is-active`):

```
<div 
  [class.is-active]="isActive">
</div>

```

This syntax also supports expressions as well for better control:

```
<div 
  [class.is-active]="isActive && isAdmin">
</div>

```

If any expression results  `false`  the  `is-active`  will be removed.

I’d highly recommend using the  `[class.class-name]`  syntax over  `[className]`  because Angular 11 handles it, the other is pretty much plain DOM bindings and no Angular 11 superpowers.

## Angular 11’s NgClass Directive

So far we’ve looked at adding just single classes, which is something the NgClass directive can also help us with as it supports  _multiple_  classes. Using multiple classes is the real reason to use the NgClass directive.

You can think of NgClass as being able to specify multiple  `[class.class-name]`  on the same element. We already learned that  `[class]`  is an  `@Input('class')`  of NgClass - so what other syntax can we use?

```
@Input('class')
klass: string

@Input()
ngClass: string | string[] | Set<string> | { [klass: string]: any; }

```

This gives us four options for setting classes in Angular 11.

The first two,  `string`  and  `string[]`:

```
<div 
  [ngClass]="'active'">
</div>
<div 
  [ngClass]="['active', 'open']">
</div>

```

It might be that you’re adding these from a theme or from user settings and wish to dynamically add an array of class names.

Ignoring  `Set<string>`, because no one uses this, let’s look at the object literal approach  `{ [klass: string]: any; }`:

```
<div 
  [ngClass]="{
    active: isActive
  }">
</div>

```

This gives us the exact same result as  `[class.active]="isActive"`  but uses the NgClass directive to toggle a class.

## Multiple CSS Classes with Angular 11 NgClass

We can also supply multiple classes inside the object:

```
<div 
  [ngClass]="{
    active: isActive,
    admin: isAdmin,
    subscriber: !isAdmin
  }">
</div>

```

And there you have it, the power of NgClass for adding css classes to your elements or components in Angular 11.

One thing I should mention is that for any kebab-case strings (such as  `is-active`) you’ll need to use quotes around the class name:

```
<div 
  [ngClass]="{
    'is-active': isActive,
    admin: isAdmin
  }">
</div>

```

It will error otherwise because it’s an object key, and object keys cannot contain hyphens unless you put them in quotes.

## Conclusion

So far we’ve covered  `[className]`,  `[class]`  and  `[ngClass]`  bindings and explored the differences between them.

My recommendation is to use  `[class.class-name]`  where appropriate to keep things simple and clean, and where more complex class manipulation is needed - use the NgClass directive!

It might be you and your team prefer to use  `[class.foo]`  style syntax for the majority of use cases, and simply adopt  `NgClass`  when introducing more complex scenarios. Or, you may favour just using  `NgClass`  - but my rule is pick one, be consistent and maintainable.
