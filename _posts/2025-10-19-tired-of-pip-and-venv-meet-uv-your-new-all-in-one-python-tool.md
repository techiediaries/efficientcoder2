---
layout: post
title: "Tired of Pip and Venv? Meet UV, Your New All-in-One Python Tool"
date: 2025-10-19
video_url: https://youtu.be/AMdG7IjgSPM?si=CagpSr0Y4GJjKWhW
categories: [New in the Dev World, Web Development]
---

Hey there, how's it going?

Let's talk about the Python world for a second. If you've been around for a while, you know the drill. You start a new project, and the ritual begins: create a directory, set up a virtual environment with `venv`, remember to activate it, `pip install` your packages, and then `pip freeze` everything into a `requirements.txt` file.

It works. It's fine. But it always felt a bit... clunky. A lot of steps. A lot to explain to newcomers.

Well, I've been playing with a new tool that's been gaining a ton of steam, and honestly? I don't think I'm going back. It's called **UV**, and it comes from Astral, the same team behind the super-popular linter, `ruff`.

The goal here is ambitious. UV wants to be the single tool that replaces `pip`, `venv`, `pip-tools`, and even `pipx`. It's an installer, an environment manager, and a tool runner all rolled into one. And because it's written in Rust, it's ridiculously fast.

So, let's walk through what a typical project setup looks like the old way... and then see how much simpler it gets with UV.

### The Old Way: The Pip & Venv Dance

Okay, so let's say we're starting a new Flask app. The old-school workflow would look something like this:

1.  `mkdir old-way-project && cd old-way-project`
2.  `python3 -m venv .venv` (Create the virtual environment)
3.  `source .venv/bin/activate` (Activate it... don't forget!)
4.  `pip install flask requests` (Install our packages)
5.  `pip freeze > requirements.txt` (Save our dependencies for later)

It's a process we've all done a hundred times. But it's also a process with a few different tools and concepts you have to juggle. For someone just starting out, it's a lot to take in.

### The New Way: Just `uv`

Now, let's do the same thing with UV.

Instead of creating a directory myself, I can just run:

`uv init new-app`

This one command creates a new directory, `cd`s into it, and sets up a modern Python project structure. It initializes a Git repository, creates a sensible `.gitignore`, and gives us a `pyproject.toml` file. This is the modern way to manage project metadata and dependencies.

But wait... where's the virtual environment? Where's the activation step?

Here's the magic. You don't have to worry about it.

Let's add Flask and Requests to our new project. Instead of `pip`, we use `uv add`:

`uv add flask requests`

When I run this, a few amazing things happen:

1.  UV sees I don't have a virtual environment yet, so it creates one for me automatically.
2.  It installs Flask and Requests into that environment at lightning speed.
3.  It updates my `pyproject.toml` file to list `flask` and `requests` as dependencies.
4.  It creates a `uv.lock` file, which records the *exact* versions of every single package and sub-dependency. This is what solves the classic "but it works on my machine!" problem.

All of that, with one command, and I never had to type `source ... activate`.

### Running Your Code (This Is the Coolest Part)

"Okay," you might be thinking, "but how do I run my code if the environment isn't active?"

Simple. You just tell UV to run it for you.

`uv run main.py`

UV finds the project's virtual environment and runs your script inside it, even though your main shell doesn't have it activated.

Now, get ready for the part that really blew my mind.

Let's say I accidentally delete my virtual environment.

`rm -rf .venv`

Normally, this would be a disaster. I'd have to recreate the environment, activate it, and reinstall everything from my `requirements.txt` file. It would be a whole thing.

But with UV? I just run the same command again:

`uv run main.py`

UV sees the environment is gone. It reads the `uv.lock` file, instantly recreates the *exact* same environment with the *exact* same packages, and then runs the code. It all happens in a couple of seconds. It's just... seamless.

If you're sharing the project with a teammate, they just clone it and run `uv sync`. That's it. Their environment is ready to go, perfectly matching yours.

### It Even Replaces Pipx for Tools

Another thing I love is how it handles command-line tools. I used to use `pipx` to install global tools like linters and formatters. UV has that built-in, too.

Want to install `ruff`?

`uv tool install ruff`

This installs it in an isolated environment but makes it available everywhere.

But even better is the `uvx` command, which lets you run a tool *without permanently installing it*.

Let's say I want to quickly check my code with `ruff` but I don't want to install it.

`uvx ruff check .`

UV will download `ruff` to a temporary environment, run the command, and then clean up after itself. It's perfect for trying out new tools or running one-off commands without cluttering your system.

### My Takeaway

I know, I know... another new tool to learn. It can feel overwhelming. But this one is different. It doesn't just add another layer; it simplifies and replaces a whole stack of existing tools with something faster, smarter, and more intuitive.

The smart caching alone is a huge win. If you have ten projects that all use Flask, UV only stores it once on your disk, saving a ton of space and making new project setups almost instantaneous.

I've fully switched my workflow over to UV, and I can't see myself going back. It just gets out of the way and lets me focus on the code.

So, what do you think? Have you tried it out? Does this sound like something that could simplify your workflow? Let me know in the comments!

