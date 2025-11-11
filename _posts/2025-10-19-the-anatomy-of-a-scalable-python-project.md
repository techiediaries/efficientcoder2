---
layout: post
title: The Anatomy of a Scalable Python Project
date: 2025-10-19
video_url: https://youtu.be/Af6Zr0tNNdE?si=GZm9iBAL1AsSBDjG
categories: [Web Development]
---

Ever start a Python project that feels clean and simple, only to have it turn into a tangled mess a few months later? Yeah, I've been there more times than I can count.

Today, I want to pull back the curtain and show you the anatomy of a Python project that’s built to last. This is the setup I use for all my production projects. It’s a blueprint that helps keep things sane, organized, and ready to grow without giving you a massive headache.

We'll walk through everything—folder structure, config, logging, testing, and tooling. The whole package.

### So, What Does "Scalable" Even Mean?

It's a word that gets thrown around a lot, right? "Scalable." But what does it actually mean in practice?

For me, it boils down to a few things:

1.  **Scales with Size:** Your codebase is going to grow. That's a good thing! It means you're adding features. A scalable structure means you don't have to constantly refactor everything just to add something new. The foundation is already there.
2.  **Scales with Your Team:** If you bring on another developer, they shouldn't need a two-week onboarding just to figure out where to put a new function. The boundaries should be clear, and the layout should be predictable.
3.  **Scales with Environments:** Moving from your local machine to staging and then to production should be... well, boring. In a good way. Your config should be centralized, making environment switching a non-event.
4.  **Scales with Speed:** Your local setup should be a breeze. Tests should run fast. Docker should just *work*. You want to eliminate friction so you can actually focus on building things.

Over the years, I've worked with everything from TypeScript to Java to C++, and while the specifics change, the principles of good structure are universal. This is the flavor that I've found works beautifully for Python.

### The Blueprint: A Balanced Folder Structure

You want just enough structure to keep things organized, but not so much that you're digging through ten nested folders to find a single file. It's a balance.

Here’s the high-level view:

```
/
├── app/      # Your application's source code
├── tests/    # Your tests
├── .env      # Environment variables (for local dev)
├── Dockerfile
├── docker-compose.yml
├── pyproject.toml
└── ... other config files
```

Right away, you see the most important separation: your `app` code and your `tests` live in their own top-level directories. This is crucial. Don't mix them.

### Diving Into the `app` Folder

This is where the magic happens. Inside `app`, I follow a simple pattern. For this example, we're looking at a FastAPI app, but the concepts apply anywhere.

```
app/
├── api/
│   └── v1/
│       └── users.py   # The HTTP layer (routers)
├── core/
│   ├── config.py    # Centralized configuration
│   └── logging.py   # Logging setup
├── db/
│   └── schema.py    # Database models (e.g., SQLAlchemy)
├── models/
│   └── user.py      # Data contracts (e.g., Pydantic schemas)
├── services/
│   └── user.py      # The business logic!
└── main.py          # App entry point
```

Let's break it down.

**`main.py` - The Entry Point**

This file is kept as lean as possible. Seriously, there's almost nothing in it. It just initializes the FastAPI app and registers the routers from the `api` folder. That's it.

**`api/` - The Thin HTTP Layer**

This is where your routes live. If you look inside `api/v1/users.py`, you won't find any business logic. You'll just see the standard GET, POST, PUT, DELETE endpoints. Their only job is to handle the HTTP request and response. They act as a thin translator, calling into the *real* logic somewhere else.

**`core/` - The Cross-Cutting Concerns**

This folder is for things that are used all over your application.
- **`config.py`**: I use Pydantic's `Settings` for this. It's amazing. You define your config as a class, and it can automatically pull in values from environment variables (like from that `.env` file). This makes managing settings for different environments a piece of cake.
- **`logging.py`**: A simple, standardized logging setup. You configure it once here, and then you can just import and use it anywhere.

**`db/` and `models/` - The Data Layers**

- **`db/schema.py`**: This is where you define your database tables, probably using something like SQLAlchemy. It describes the *shape* of your data in the database.
- **`models/user.py`**: These are your Pydantic models that define the *contracts* for your API. What JSON should a user send to create a user? What JSON will they get back? This is where you define that, and you get free data validation out of it.

**`services/` - The Heart of Your Application**

This is the most important folder, in my opinion. This is where your actual business logic lives. The `UserService` takes a database session and does the real work: querying for users, creating a new user, running validation logic, etc.

Why is this so great?
- Your API layer stays clean and simple.
- You can test your business logic directly, without needing to spin up a web server.
- Want to switch from PostgreSQL to a different database? Or even an external API? You only have to change it here. The rest of your app doesn't care.

### Let's Talk About Testing

Your `tests` folder should mirror your `app` folder's structure. This makes it incredibly easy to find the tests for any given piece of code.

```
tests/
└── api/
    └── v1/
        └── test_users.py
```

For testing, I use an in-memory SQLite database. This keeps my tests completely isolated from my production database and makes them run super fast.

FastAPI has a fantastic dependency injection system that makes testing a dream. In my tests, I can just "override" the dependency that provides the database session and swap it with my in-memory test database. Now, when I run a test that hits my API, it's running against a temporary, clean database every single time.

### Tooling That Ties It All Together

- **`pyproject.toml`**: This is where your project dependencies and settings (like for `pytest`) live. I use `uv` these days—it's incredibly fast.
- **`Dockerfile` & `docker-compose.yml`**: This is how you guarantee that your local development environment is *exactly* like your production environment. When I run `docker-compose up`, it spins up my app in a container, using the same `Dockerfile` that will eventually be deployed to the cloud. No more "but it works on my machine!"
- **`.env` file**: This holds your local environment variables, like database passwords. Crucially, you **never** commit this file to Git. It's for your machine only.

### How It All Flows Together

So, let's trace a request:

1.  A `GET /users` request hits the router in `api/v1/users.py`.
2.  FastAPI's dependency injection system automatically creates a `UserService` instance, giving it a fresh database session.
3.  The route calls the `list_users` method on the service.
4.  The service runs a query against the database, gets the results, and returns them.
5.  The router takes those results, formats them as a JSON response, and sends it back to the client.

The beauty of this is the clean separation of concerns. The API layer handles HTTP. The service layer handles business logic. The database layer handles persistence.

This structure lets you start small and add complexity later without making a mess. The boundaries are clear, which makes development faster, testing easier, and onboarding new team members a whole lot smoother.

Of course, this is a starting point. You might need a `scripts/` folder for data migrations or other custom tasks. But this foundation... it's solid. It's been a game-changer for me, and I hope it can be for you too.
