---
layout: post
title: "[3] Using NVM on Windows"
image: "images/content/updating-angular-cli-projects.png"
excerpt: "How to use NVM on Windows" 
categories: [webdev]
permalink: /nvm-windows/
previousLink: /use-nodejs-run-javascript-servers/
tags : [webdev, nodejs , javascript] 
---

You can use the following command to check the current version and verify that you have Node.js installed on your system:

```bash
node -v
```
The terminal displays "v14.0.0" for our case.

The official repository can be accessed at [https://github.com/nvm-sh/nvm](https://github.com/nvm-sh/nvm) for more details on the available commands and their usage.

The nvm utility can run on Linux, macOS, and Windows Subsystem for Linux (WSL). Two unofficial Windows alternatives are available: 

- nvm-windows (available at https://github.com/coreybutler/nvm-windows) 
- and nodist (available at https://github.com/marcelklehr/nodist).

You can now use Node.js packages like Express.js to build your backend application and frontend libraries and tools like Angular CLI on your development machine.

In the next tutorials, we'll be using npm to get our backend and frontend app dependencies set up.

