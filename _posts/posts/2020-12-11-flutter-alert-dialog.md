---
layout: post
title:  "Flutter Dialog: Alert, Custom and Full-Screen Dialogs"
---

In this article, we'll see how to create and use dialogs in Flutter.

In the mobile apps, we use Dialog everywhere from alerting some situation to the user to get some feedback from the user.

**We can use alert dialog to pause the user interaction with the current screen and show some intermediate action to the user like show error, take user email to process next step kind of stuff.**

When comes to the Dialog in Flutter there are multiple ways of implement like

1.  Alert Dialog
2.  Custom Dialog
3.  Full-Screen Dialog

## Flutter simple Alert Dialog

Adding simple Dialog to your screen in pretty easy in Flutter.

Before adding Dialog you must call  **showDialog**  function to change current screen state to show the intermediate  **Dialog**  popup.

In here we use  **AlertDialog**  widget to show simple **Dialog** with title and some text in the body.

```dart

      showDialog(
              context: context,
              builder: (BuildContext context){
                  return AlertDialog(
                    title: Text("Alert Dialog"),
                    content: Text("Dialog Content"),
                  );
              }
            )

```

Under the  **content**, it  **does not required**  to add only the Text, you can add any widget like Image or something. But Because we use Alert Dialogs in simple use case it better to show simple text or an image inside the content.

## Show Dialog when click button

you can add above implementation inside the onTap method or you can add implementation to separate method and call the method inside the onTap.

```dart

floatingActionButton: FloatingActionButton(
        onPressed: () {
          showDialog(
              context: context,
              builder: (BuildContext context) {
                return AlertDialog(
                  title: Text("Alert Dialog"),
                  content: Text("Dialog Content"),
                );
              });
        },
        child: Icon(Icons.add),
      ), 

```

### Add buttons to Alert Dialogs

In the AlertDialog widget, there is a parameter called  **action**. ​It accepts arrays of widgets and you can provide multiple buttons to that. Those Buttons will appear in the  **bottom right corner**  of the dialog.

```dart
           actions:[
                      FlatButton(
                        child: Text("Close"),
                      )
                    ],
```

### How to close Alert Dialogs

In here we need to close the current Dialog when click Close button.

We can use pop method in Navigator class for that.

```dart
           FlatButton(
                        child: Text("Close"),
                        onPressed: (){
                          Navigator.of(context).pop();
                        },
                      )
```

## Flutter custom Alert Dialog

Simple Alert Dialog will not be useful for every requirement. If you want to implement more advanced custom Dialog, you can use  **Dialog**  widget for that. Instead of the  **AlertDialog**  , in here we return  **Dialog**  widget. The  **showDialog**  method will remain the same.

You can use a  **Container**  widget to set relevant height for the  **Dialog**.

Set the round corner to the Dialog by setting  **RoundedRectangleBorder**  for the shape and provide radius as you need.

```dart
           
showDialog(
        context: context,
        builder: (BuildContext context) {
          return Dialog(
            shape: RoundedRectangleBorder(
                borderRadius:
                    BorderRadius.circular(20.0)), //this right here
            child: Container(
              height: 200,
              child: Padding(
                padding: const EdgeInsets.all(12.0),
                child: Column(
                  mainAxisAlignment: MainAxisAlignment.center,
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    TextField(
                      decoration: InputDecoration(
                          border: InputBorder.none,
                          hintText: 'What do you want to remember?'),
                    ),
                    SizedBox(
                      width: 320.0,
                      child: RaisedButton(
                        onPressed: () {},
                        child: Text(
                          "Save",
                          style: TextStyle(color: Colors.white),
                        ),
                        color: const Color(0xFF1BC0C5),
                      ),
                    )
                  ],
                ),
              ),
            ),
          );
        });

```

## Flutter Full Screen Dialog

By Using  **showDialog**  method we cannot show full-screen Dialog. If we want to show dialog in full screen we must use  **showGeneralDialog** method provided by Flutter SDK.

There are some interesting parameter available in  **showGeneralDialog** method.

**barrierColor**  – Change dropshadow outside the dialog.

**transitionDuration**  – Animation duration

**barrierDismissible**  – Dismiss dialog by clicking outside in current route

```dart
           
showGeneralDialog(
      context: context,
      barrierDismissible: true,
      barrierLabel: MaterialLocalizations.of(context)
          .modalBarrierDismissLabel,
      barrierColor: Colors.black45,
      transitionDuration: const Duration(milliseconds: 200),
      pageBuilder: (BuildContext buildContext,
          Animation animation,
          Animation secondaryAnimation) {
        return Center(
          child: Container(
            width: MediaQuery.of(context).size.width - 10,
            height: MediaQuery.of(context).size.height -  80,
            padding: EdgeInsets.all(20),
            color: Colors.white,
            child: Column(
              children: [
                RaisedButton(
                  onPressed: () {
                    Navigator.of(context).pop();
                  },
                  child: Text(
                    "Save",
                    style: TextStyle(color: Colors.white),
                  ),
                  color: const Color(0xFF1BC0C5),
                )
              ],
            ),
          ),
        );
      });

```

## Conclusion

In this article, we've seen how to implement AlertDialog in Flutter and How to convert that to more customized version and how to show dialog in full screen. 
