---
layout: post
title: Python 3.14 is Here, and It's More Than Just a Number
date: 2025-10-19
video_url: https://youtu.be/S_shhw4VV68?si=aXZeYwJn4ZpcjTHc
categories: [New in the Dev World, Web Development]
---

Python 3.14. It's finally here.

I feel like I've been waiting for this my whole career. It just feels... right. But now that it's here, I'm also a little bit sad. What do we wait for now? Python 3.162, the square root of 10? It doesn't have the same ring to it.

Or maybe Python 6.28... you know, 2π. That's probably not happening in my lifetime. Unless AI makes us all immortal, and I'm still here in 500 years making Python videos. Who knows? Maybe AI will be doing all the coding for us by then, or maybe we'll all have moved on to Rust once it becomes as easy as Python.

I have no idea. But let's stick with the here and now, shall we?

Python 3.14 has some genuinely nice new features. What actually matters to you will depend on what kind of developer you are. We all use Python in our own weird and wonderful ways, right?

So, I'm not going to give you a laundry list of every single change. Instead, I'm going to show you four features that I personally love in 3.14, complete with some code examples.

Oh, and before I forget, the new shell has syntax highlighting! If I open up the old Python 3.13 shell and type, it's just plain text. But in the new 3.14 shell... look at that! Colors! It's a small thing, but I really, really like it.

Okay, let's dive into the four things that I think are worth highlighting.

### 1. A Tiny Tweak to Exception Syntax

First up, a quality-of-life improvement for handling exceptions.

You know how when you want to catch multiple exception types, you have to wrap them in parentheses? Like this:

```python
try:
    # some risky code
except (ValueError, KeyError) as e:
    print("Caught one of these two errors.")
```

Well, in Python 3.14, you no longer need those parentheses. You can just... not type them.

```python
try:
    # some risky code
except ValueError, KeyError as e:
    print("Caught one of these two errors.")
```

My code editor's AI hasn't caught on yet and tries to "fix" it for me. Nope, GitHub Copilot, we're living in the future now.

It's a tiny change, I know. But think about it. How many times have you typed those parentheses? As developers, every keystroke on our mechanical keyboards costs energy. This change probably saves us about 17 calories a year. That's enough for one bite of a donut.

I'll take it. In fact, I already did. So I guess I have to switch to 3.14 now.

### 2. Built-in Zstandard Compression

The second thing I'm excited about is that `zstandard` compression is now built into Python.

This is a really fast compression algorithm. I put together a quick benchmark comparing it to `gzip` and `bzip2`, just compressing some random data.

And wow. `zstandard` is not only super fast at compression, but it also creates a much smaller file than `gzip`. Decompression is a little slower than `gzip`, but the size difference is significant. And when you compare it to `bzip2`... the difference is even bigger.

In the past, you might have chosen `bzip2` over `gzip` for its smaller file sizes, but you'd take a big hit on performance. Now, `zstandard` gives you the best of both worlds. This is perfect for things like caching, log files, or anywhere you need both speed and small size.

### 3. Template Strings: f-strings on Steroids

This one is a bit more advanced, but it's incredibly powerful. Template strings are like a generalization of f-strings.

The thing with f-strings is that they are interpolated immediately. You write it, Python processes it, and that's that. You have no control over *how* it gets processed.

Templates are different. They don't return a string directly; they return a `Template` object. This object lets you inspect the parts to be interpolated and apply your own custom logic.

Why is this useful? Think about safe HTML rendering or sanitizing inputs.

In this example, I'm using a template string (notice the `t` prefix) to inject some "evil" script code.

```python
# evil_script = "<script>alert('pwned')</script>"
# message = t"Hello, {evil_script}!"
```

With a custom function, I can process this template and tell it to sanitize the inputs, escaping the HTML. So instead of the script actually running, you get a harmless, escaped version of it. This is huge for preventing injection attacks.

You probably won't be using template strings in your day-to-day code, but you'll definitely start seeing them in libraries for things like web frameworks, logging, and anywhere that security and custom formatting are important.

### 4. The Future is Now for Annotations

Finally, my favorite change: deferred annotations are now the default.

If you've ever had a class that needs to reference its own type in a type hint, you know the pain. For example, a `Node` class that has a `next` attribute, which is also a `Node`.

```python
class Node:
    next: Node | None
```

In older Python versions, this would throw a `NameError` because `Node` hasn't been fully defined yet. The solution was to either write it as a string (`'Node'`) or add this to the top of your file:

`from __future__ import annotations`

Well, you can forget about that now. In Python 3.14, it just works. No extra import needed. The future is not what it used to be... it's here.

This means less boilerplate, cleaner files, and just a smoother experience.

Even better, there's a new `annotationlib` module that lets you inspect annotations without actually evaluating them. This is a massive deal for libraries like Pydantic and FastAPI that rely heavily on type introspection.

---

So there you have it. For me, the cleaner exception syntax, powerful compression, safer templates, and no-fuss annotations make Python 3.14 a really solid and practical upgrade.

But I'd love to hear what you think. Are you excited about template strings? The performance gains from sub-interpreters? Or are you just happy the version number finally lines up with Pi? Let me know.