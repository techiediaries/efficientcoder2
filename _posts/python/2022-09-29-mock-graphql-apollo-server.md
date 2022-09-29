---
layout: post
title: "Add days to dates in Python"
image: "images/content/updating-angular-cli-projects.png"
excerpt: "Learn how to add days to dates in Python." 
categories: [python]
permalink: /add-days-dates-python/
tags : [python] 
---

In this example, we'll see how to add days to dates in python. 

To add days to a date in python, you simply need to use the `timedelta()` class that you should import from the `datetime` module.

For example:

```python
dateplustwo = aDate + timedelta(days=2). 
```

We passed a days argument to the `timedelta` class and added two days to the date.

Here is the complete working example:

```python
>>> from datetime import datetime, date, timedelta
>>> today = datetime.today()
>>> print(today + timedelta(days=1))
2022-09-30 20:42:58.812066
```

The [`datetime.today()`](https://docs.python.org/3/library/datetime.html#datetime.datetime.today) method returns the current local datetime.

This can also be used with `date` instead of `datetime` as follows:

```python
>>> today  = date.today()
>>> print(today + timedelta(days=3))
2022-10-02
```
The [date.today](https://docs.python.org/3/library/datetime.html#datetime.date.today) method returns a date object that represents the current local date.

Don't forget to import the datetime or date and timedelta classes from the datetime module.

We can also use a date string but we first need to convert it a datetime object before adding days to it. Here is a working example:

```python
>>> today = '2022-09-29' # format: yyyy-mm-dd
>>> datetime.strptime(today, '%Y-%m-%d')
datetime.datetime(2022, 9, 29, 0, 0)
>>> print(today)
2022-09-29
>>> myday = datetime.strptime(today, '%Y-%m-%d')
>>> print(myday + timedelta(days=2))
2022-10-01 00:00:00
```

The [datetime.timedelta](https://docs.python.org/3/library/datetime.html#datetime.timedelta) class takes the days we need to add to the date or datetime objects.

We formatted the date string as `yyyy-mm-dd` and we used the `strptime()` method to convert the string to a datetime object, corresponding to the provided format, that we can use with the `timedelta()` class.
