---
layout: post
title:  "Posting a Google Form to a Webhook"
date:   2019-06-24
---

In this tutorial, we'll learn how to create a script for getting Google form submissions programmatically to integrate Google Forms with Webhooks. 

If you are familiar with JavaScript, you need to write a Google Apps script and set up a trigger that will be invoked every time we submit the form. 

Let's see this step by step.

Heab back to your Google form, and click on the three vertical dots in the top right corner and select “Script Editor”.


![](https://miro.medium.com/max/369/1*e8tXee_HGPcVefK-63txOg.png)

Next, copy the following script into the editor:

```js
var webhook = "enter your URL here";
function onSubmit(event) {
    const form = FormApp.getActiveForm();
    const allResponses = form.getResponses();
    const latestResponse = allResponses[allResponses.length - 1];
    var res = latestResponse.getItemResponses();
    var data = {};
    for (var i = 0; i < res.length; i++) {
        const qTitle = res[i].getItem().getTitle();
        var qAnswer = res[i].getResponse();
        data[qTitle] = qAnswer;
    }
UrlFetchApp.fetch(webhook, {
        "method": "post",
        "contentType": "application/json",
        "payload": JSON.stringify(data)
    });
};
```

Make sure to replace, the `webhook` variable  with your own service URL.

Now, this is an important step, to actually get the form submissions; we need to set up a trigger. Click on **Edit** and select **Current Project’s Triggers**


![](https://miro.medium.com/max/235/1*hitCBqMNw8w9nOqojBkzXg.png)

Next, create a new trigger using the **Add Trigger** button on the bottom right-hand side.

You'll be presented with the following window:


![](https://miro.medium.com/max/723/1*QAibNzfutqebzvle0vhvsQ.png)

Simply make sure that **Select event type** is set to **On form submit** then click **Save**.

That's it, if you go and submit your form, you will receive the data as a JSON response.

