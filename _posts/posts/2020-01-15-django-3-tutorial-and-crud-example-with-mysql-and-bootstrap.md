---
layout: post
title: "Django 3 Tutorial & CRUD Example with MySQL and Bootstrap"
image: "images/content/learn-django.png"
excerpt: "A Complete step by step tutorial to learn Django from scratch" 
tags : [ python , django, mysql ]
date: 2021-02-25
---

Django 3 is released with full async support! In this tutorial, we'll see by example how to create a CRUD application from scratch and step by step. We'll see how to configure a MySQL database, enable the admin interface, and create the django views. 

We'll be using Bootstrap 4 for styling.

You'll learn how to:

- Implement CRUD operations, 
- Configure and access a MySQL database, 
- Create django views, templates and urls,
- Style the UI with Bootstrap 4


## Django 3 Features

Django 3 comes with many new features such as:

- MariaDB support: Django now officially supports MariaDB 10.1+. You can use MariaDB via the MySQL backend,
- ASGI support for async programming,
- Django 3.0 provides support for running as an ASGI application, making Django fully async-capable
- Exclusion constraints on PostgreSQL: Django 3.0 adds a new ExclusionConstraint class which adds exclusion constraints on PostgreSQL, etc.


## Prerequisites

Let's start with the prerequisites for this tutorial. In order to follow the tutorial step by step, you'll need a few requirements, such as:

- Basic knowledge of Python,
- Working knowledge of Django (`django-admin.py` and `manage.py`),
- A recent version of Python 3 installed on your system  (the latest version is **3.7**),
- MySQL database installed on your system.

We will be using `pip` and `venv` which are bundled as modules in recent versions of Python so you don't actually need to install them unless you are working with old versions.

If you are ready, lets go started!

## Django 3 Tutorial, Step 1 - Creating a MySQL Database

In this step, we'll create a mysql database for storing our application data.

Open a new command-line interface and run the `mysql` client as follows:

```bash
$ mysql -u root -p
```

You'll be prompted for your MySQL password, enter it and press **Enter**.

Next, create a database using the following SQL statement:

```bash
mysql> create database mydb;
```

We now have an empty mysql database!

## Step 2 - Initializing a New Virtual Environment

In this step, we'll initialize a new virtual environment for installing our project packages in separation of the system-wide packages. 

Head back to your command-line interface and run the following command:

```bash
$ python3 -m venv .env
``` 

Next, activate your virtual environment using the following command:

```bash
$ source .env/bin/activate
```

At this point of our tutorial, we've a mysql database for persisting data and created a virtual environment for installing the project packages. 

## Step 3 - Installing Django and MySQL Client

In this step, we'll install django and mysql client from PyPI using `pip` in our activated virtual environment.

Head back to your command-line interface and run the following command to install the django package:

```bash
$ pip install django
```

At the time of writing this tutorial, `django-3.0.2` is installed.

You will also need to install the mysql client for Python using `pip`:

```bash
$ pip install mysqlclient
```

## Step 4 - Initializing a New Project

In this step, we'll initialize a new django project using the `django-admin`.

Head back to your command-line interface and run the following command:

```bash
$ django-admin startproject djangoCrudExample
```

Next, open the `settings.py` file and update the database settings to configure the `mydb` database:

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql', 
        'NAME': 'mydb',
        'USER': 'root',
        'PASSWORD': '<YOUR_DB_PASSWORD>',
        'HOST': 'localhost',   
        'PORT': '3306',
    }    
}
```

Next, migrate the database using the following commands:

```bash
$ cd djangoCrudExample
$ python3 manage.py migrate
```

You'll get a similar output:

```bash
Operations to perform:
  Apply all migrations: admin, auth, contenttypes, sessions
Running migrations:
  Applying contenttypes.0001_initial... OK
  Applying auth.0001_initial... OK
  Applying admin.0001_initial... OK
  Applying admin.0002_logentry_remove_auto_add... OK
  Applying admin.0003_logentry_add_action_flag_choices... OK
  Applying contenttypes.0002_remove_content_type_name... OK
  Applying auth.0002_alter_permission_name_max_length... OK
  Applying auth.0003_alter_user_email_max_length... OK
  Applying auth.0004_alter_user_username_opts... OK
  Applying auth.0005_alter_user_last_login_null... OK
  Applying auth.0006_require_contenttypes_0002... OK
  Applying auth.0007_alter_validators_add_error_messages... OK
  Applying auth.0008_alter_user_username_max_length... OK
  Applying auth.0009_alter_user_last_name_max_length... OK
  Applying auth.0010_alter_group_name_max_length... OK
  Applying auth.0011_update_proxy_permissions... OK
  Applying sessions.0001_initial... OK
```

This simply applies a set of builtin django migrations to create some necessary database tables or the working of django.

## Step 5 - Installing `django-widget-tweaks`

In this step, we'll install `django-widget-tweaks` in our virtual environment.
Head back to your command-line interface and run the following command: 

```bash
$ pip insll django-widget-tweaks
```

Next, open the `settings.py` file and add the application to the installed apps:

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'widget_tweaks'
]
```



## Step 6 -  Creating an Admin User

In this step, we'll create an admin user that will allow us to access the admin interface of our app using the following command:

```bash
$ python manage.py createsuperuser
```

Provide the desired username, email and password when prompted:

```bash
Username (leave blank to use 'ahmed'): 
Email address: ahmed@gmail.com
Password: 
Password (again): 
Superuser created successfully.
```

## Step 7 -  Creating a Django Application

In this step, we'll create a django application.

Head back to your command-line interface, and run the following command:

```bash
$ python manage.py startapp crudapp
```

Next, you need to add it in the `settings.py` file as follows:

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'widget_tweaks',
    'crudapp'
]
```

## Step 8 - Creating the Django Model(s)

In this step. we'll create the database model for storing contacts.

Open the `crudapp/models.py` file and add the following code:

```python
from django.db import models


class Contact(models.Model):
    firstName = models.CharField("First name", max_length=255, blank = True, null = True)
    lastName = models.CharField("Last name", max_length=255, blank = True, null = True)
    email = models.EmailField()
    phone = models.CharField(max_length=20, blank = True, null = True)
    address = models.TextField(blank=True, null=True)
    description = models.TextField(blank=True, null=True)
    createdAt = models.DateTimeField("Created At", auto_now_add=True)
    
    def __str__(self):
        return self.firstName

```

After creating these model, you need to create migrations using the following command:

```bash
$ python manage.py makemigrations

```

You should get a similar output:

```bash
  crudapp/migrations/0001_initial.py
    - Create model Contact

```

Next, you need to migrate your database using the following command:

```bash
$ python manage.py migrate
```

You should get a similar output:

```bash
  Applying crudapp.0001_initial... OK
```

## Step 9 - Creating a Form

In this step, we'll create a form for creating a contact.

In the `crudapp` folder, create a `forms.py` file and add the following code:


```py
from django import forms
from .models import Contact

class ContactForm(forms.ModelForm):
    class Meta:
        model = Contact
        fields = "__all__"

```

We import the Contact model from the `models.py` file. We created a class called `ContactForm`, subclassing Django’s ModelForms from the `django.forms` package and specifying the model we want to use. We also specified that we will be using all fields in the `Contact` model. This will make it possible for us to display those fields in our templates.

## Step 10 - Creating the Views

In this step, we'll create the views for performing the CRUD operations.

Open the `crudapp/views.py` file and add:

```py
from django.shortcuts import render, redirect, get_object_or_404
from .models import Contact
from .forms import ContactForm
from django.views.generic import ListView, DetailView
```

Next, add: 

```py
class IndexView(ListView):
    template_name = 'crudapp/index.html'
    context_object_name = 'contact_list'
    
    def get_queryset(self):
        return Contact.objects.all()

class ContactDetailView(DetailView):
    model = Contact
    template_name = 'crudapp/contact-detail.html'
```

Next, add:


```py
def create(request):
    if request.method == 'POST':
        form = ContactForm(request.POST)
        if form.is_valid():
            form.save()
            return redirect('index')
    form = ContactForm()

    return render(request,'crudapp/create.html',{'form': form})

def edit(request, pk, template_name='crudapp/edit.html'):
    contact = get_object_or_404(Contact, pk=pk)
    form = ContactForm(request.POST or None, instance=post)
    if form.is_valid():
        form.save()
        return redirect('index')
    return render(request, template_name, {'form':form})

def delete(request, pk, template_name='crudapp/confirm_delete.html'):
    contact = get_object_or_404(Contact, pk=pk)
    if request.method=='POST':
        contact.delete()
        return redirect('index')
    return render(request, template_name, {'object':contact})
```

## Step 11 - Creating Django Templates

Open the `settings.py` file and add `os.path.join(BASE_DIR, 'templates')` to the `TEMPLATES` array:

```py
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [os.path.join(BASE_DIR, 'templates')],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
```

This will tell django to look for the templates in the `templates` folder.

Next, inside the `crudapp` folder create a `templates` folder:

```bash
$ mkdir templates
```

Next, inside the `templates` folder, create the following files:

- `base.html`
- `confirm_delete.html`
- `edit.html`
- `index.html`
- `create.html`
- `contact-detail.html`

By running the following commands from the root of your project:

```bash
$ mkdir templates
$ cd templates
$ mkdir crudapp
$ touch crudapp/base.html
$ touch crudapp/confirm_delete.html
$ touch crudapp/edit.html
$ touch crudapp/index.html
$ touch crudapp/create.html
$ touch crudapp/contact-detail.html
```

Open the `crudapp/templates/base.html` file and the add:

{% raw %}
```html
<!DOCTYPE html>
<html>
<head>
 <title>Django 3 CRUD Example</title>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.4.0/css/bootstrap.min.css">
</head>
<body>
{% block content %}
{% endblock %}
<script src="https://code.jquery.com/jquery-3.3.1.slim.min.js" integrity="sha384-q8i/X+965DzO0rT7abK41JStQIAqVgRVzpbzo5smXKp4YfRvH+8abtTE1Pi6jizo" crossorigin="anonymous"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.3/umd/popper.min.js" integrity="sha384-ZMP7rVo3mIykV+2+9J3UJ46jBk0WLaUAdn689aCwoqbBJiSnjAK/l8WvCWPIPm49" crossorigin="anonymous"></script>
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.4.0/js/bootstrap.min.js"></script>
</body>
</html>
```
{% endraw %}

Next, open the `crudapp/templates/index.html` file and the add:

{% raw %}
```html
{% extends 'crudapp/base.html' %}
{% block content %}
<div class="container-fluid">
    <div class="row">
        <div class="col-md-1 col-xs-1 col-sm-1"></div>
        <div class="col-md-10 col-xs-10 col-sm-10">
            <h3 class="round3" style="text-align:center;">Contacts</h3>
        </div>
        <div class="col-md-1 col-xs-1 col-sm-1"></div>
    </div>
    <div class="row">
        <div class="col-md-10 col-xs-10 col-sm-10"></div>
        <div class="col-md-2 col-xs-1 col-sm-1">
            <br />
            <a href="{% url 'create' %}">
                <button type="button" class="btn btn-success">
                    <span class="glyphicon glyphicon-plus"></span>
                </button>
            </a>
        </div>
    </div>
    <br />
    {% for contact in contact_list %}
    <div class="row">
        <div class="col-md-1 col-xs-1 col-sm-1"></div>
        <div class="col-md-7 col-xs-7 col-sm-7">
            <ul class="list-group">

                <li class="list-group-item ">
                    <a href="{% url 'detail' contact.pk %}"> {{ contact.firstName }} {{contact.lastName}} </a> <span class="badge"></span>
                </li>
            </ul>
            <br>
        </div>
        <div class="col-md-1 col-xs-1 col-sm-1">
            <a href="{% url 'detail' contact.pk %}">
                <button type="button" class="btn btn-info">
                    <span class="glyphicon glyphicon-open"></span>
                </button>
            </a>
        </div>
        <div class="col-md-1">
            <a href="{% url 'edit' contact.pk %}">
                <button type="button" class="btn btn-info">
                    <span class="glyphicon glyphicon-pencil"></span>
                </button>
            </a>
        </div>
        <div class="col-md-1">
            <a href="{% url 'delete' contact.pk %}">
                <button type="button" class="btn btn-danger">
                    <span class="glyphicon glyphicon-trash"></span>
                </button>
            </a>
        </div>
        <div class="col-md-1 col-xs-1 col-sm-1"></div>
    </div>
    {% endfor %}
</div>
{% endblock %}
```
{% endraw %}


Next, open the `crudapp/templates/create.html` file and the add:


{% raw %}

```
{% load widget_tweaks %}
<!DOCTYPE html>
<html>

<head>
    <title>Posts</title>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.1.3/css/bootstrap.min.css"
        integrity="sha384-MCw98/SFnGE8fJT3GXwEOngsV7Zt27NXFoaoApmYm81iuXoPkFOJwJ8ERdknLPMO" crossorigin="anonymous">
    <style type="text/css">
        <style>
    </style>
    </style>

</head>

<body>

    <div class="container-fluid">
        <div class="row">
            <div class="col-md-1 col-xs-1 col-sm-1"></div>

            <div class="col-md-10 col-xs-10 col-sm-10 ">
                <br />
                <h6 style="text-align:center;">
                    <font color="red"> All fields are required</font>
                </h6>
            </div>
            <div class="col-md-1 col-xs-1 col-sm-1">
            </div>
        </div>
        <div class="row">
            <div class="col-md-1 col-xs-1 col-sm-1"></div>
            <div class="col-md-10 col-xs-10 col-sm-10">
                <form method="post" novalidate>
                    {% csrf_token %}
                    {% for hidden_field in form.hidden_fields %}
                    {{ hidden_field }}
                    {% endfor %}
                    {% for field in form.visible_fields %}
                    <div class="form-group">
                        {{ field.label_tag }}
                        {% render_field field class="form-control" %}
                        {% if field.help_text %}
                        <small class="form-text text-muted">{{ field.help_text }}</small>
                        {% endif %}
                    </div>
                    {% endfor %}
                    <button type="submit" class="btn btn-primary">post</button>
                </form>
                <br>
            </div>
            <div class="col-md-1 col-xs-1 col-sm-1"></div>
        </div>
    </div>
    <script src="https://code.jquery.com/jquery-3.3.1.slim.min.js"
        integrity="sha384-q8i/X+965DzO0rT7abK41JStQIAqVgRVzpbzo5smXKp4YfRvH+8abtTE1Pi6jizo"
        crossorigin="anonymous"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.3/umd/popper.min.js"
        integrity="sha384-ZMP7rVo3mIykV+2+9J3UJ46jBk0WLaUAdn689aCwoqbBJiSnjAK/l8WvCWPIPm49"
        crossorigin="anonymous"></script>
    <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.1.3/js/bootstrap.min.js"
        integrity="sha384-ChfqqxuZUCnJSK3+MXmPNIyE6ZbWh2IMqE241rYiqJxyMiZ6OW/JmZQ5stwEULTy"
        crossorigin="anonymous"></script>
</body>

</html>
```
{% endraw %}

Next, open the `crudapp/templates/edit.html` file and the add:

{% raw %}

```html
{% load widget_tweaks %}
<!DOCTYPE html>
<html>

<head>
    <title>Edit Contact</title>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.1.3/css/bootstrap.min.css"
        integrity="sha384-MCw98/SFnGE8fJT3GXwEOngsV7Zt27NXFoaoApmYm81iuXoPkFOJwJ8ERdknLPMO" crossorigin="anonymous">
    <style type="text/css">
        <style>
    </style>
    </style>

</head>

<body>
<div class="container-fluid">
    <div class="row">
        <div class="col-md-1 col-xs-1 col-sm-1"></div>
        <div class="col-md-10 col-xs-10 col-sm-10 ">
            <br />
            <h6 style="text-align:center;">
                <font color="red"> All fields are required</font>
            </h6>
        </div>
        <div class="col-md-1 col-xs-1 col-sm-1">
        </div>
    </div>
    <div class="row">
        <div class="col-md-1 col-xs-1 col-sm-1"></div>
        <div class="col-md-10 col-xs-10 col-sm-10">
            <form method="post" novalidate>
                {% csrf_token %}
                {% for hidden_field in form.hidden_fields %}
                {{ hidden_field }}
                {% endfor %}
                {% for field in form.visible_fields %}
                <div class="form-group">
                    {{ field.label_tag }}
                    {% render_field field class="form-control" %}
                    {% if field.help_text %}
                    <small class="form-text text-muted">{{ field.help_text }}</small>
                    {% endif %}
                </div>
                {% endfor %}
                <button type="submit" class="btn btn-primary">submit</button>
            </form>
            <br>
        </div>
        <div class="col-md-1 col-xs-1 col-sm-1"></div>
    </div>
</div>
<script src="https://code.jquery.com/jquery-3.3.1.slim.min.js" integrity="sha384-q8i/X+965DzO0rT7abK41JStQIAqVgRVzpbzo5smXKp4YfRvH+8abtTE1Pi6jizo" crossorigin="anonymous"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.3/umd/popper.min.js" integrity="sha384-ZMP7rVo3mIykV+2+9J3UJ46jBk0WLaUAdn689aCwoqbBJiSnjAK/l8WvCWPIPm49" crossorigin="anonymous"></script>
<script src="https://stackpath.bootstrapcdn.com/bootstrap/4.1.3/js/bootstrap.min.js" integrity="sha384-ChfqqxuZUCnJSK3+MXmPNIyE6ZbWh2IMqE241rYiqJxyMiZ6OW/JmZQ5stwEULTy" crossorigin="anonymous"></script>
</body>
</html>
```
{% endraw %}

Next, open the `crudapp/templates/confirm_delete.html` file and the add:

{% raw %}

```html
{% extends 'crudapp/base.html' %}
{% block content %}
<div class="container">
    <div class="row"></div>
    <br />
    <div class="row">
        <div class="col-md-2 col-xs-2 col-sm-2"></div>
        <div class="col-md-10 col-xs-10 col-sm-10">
            <form method="post">
                {% csrf_token %}
                <div class="form-row">
                    <div class="alert alert-warning">
                        Are you sure you want to delete {{ object }}?
                    </div>
                </div>
                <button type="submit" class="btn btn-danger">
                    <span class="glyphicon glyphicon-trash"></span>
                </button>
            </form>
        </div>
    </div>
</div>
{% endblock %}
```
{% endraw %}

## Step 12 - Creating URLs

In this step, we'll create the urls to access our CRUD views. 

Go to the `urls.py` file and update it as follows:

```python
from django.contrib import admin
from django.urls import path
from crudapp import views

urlpatterns = [
    path('admin/', admin.site.urls),
    path('contacts/', views.IndexView.as_view(), name='index'),
    path('contacts/<int:pk>/', views.ContactDetailView.as_view(), name='detail'),
    path('contacts/edit/<int:pk>/', views.edit, name='edit'),
    path('contacts/create/', views.create, name='create'),
    path('contacts/delete/<int:pk>/', views.delete, name='delete'),
]
```

## Step 11 - Running the Local Development Server

In this step, we'll run the local development server for playing with our app without deploying it to the web.

Head back to your command-line interface and run the following command:

```bash
$ python manage.py runserver
```

Next, go to the `http://localhost:8000/` address with a web browser.


## Conclusion 

In this django 3 tutorial, we have initialized a new django project, created and migrated a MySQL database, and built a simple CRUD interface.
