---
layout: post
title:  "Add and Customize Borders In Flutter"
---

In this article, we'll see how add and customize borders in Flutter.

Adding border and customization is easy in Flutter because of  [BoxDecoration](https://api.flutter.dev/flutter/painting/BoxDecoration-class.html)  widget provided by flutter. In this article, I will demonstrate how to add a different border to the container using BoxDecoration widget

## Add simple border with color

you can set decoration border property to set the color of the border.

    `Container(
                padding: EdgeInsets.all(10),
                decoration: BoxDecoration(
                  border: Border.all(color: Colors.blueAccent)
                ),
                child: Text('Simple Border'),
              ),` 


## Change width of the border

All factory method in Border class have width property to set the width of the border

    `Container(
                padding: EdgeInsets.all(10),
                decoration: BoxDecoration(
                  border: Border.all(color: Colors.blueAccent,width: 10)
                ),
                child: Text('Simple Border'),
              )` 


## Add different borders for each side

You can use Border class instead of all factory method to set a different border to each side in the widget.

    `Container(
                padding: EdgeInsets.all(10),
                decoration: BoxDecoration(
                  border: Border(
                    top: BorderSide(
                      width: 10,
                      color: Colors.amber
                    ),
                    bottom: BorderSide(
                      color: Colors.blue
                    )
                  )
                ),
                child: Text('Simple Border'),
              )` 


## Round corner border/ Border radius

You can set borderRadius property in BoxDecoration class to give radius to the borders. You can specify the same radius for all the corners or different radius for each corner

## Equal radius for all corners

    `Container(
                padding: EdgeInsets.all(10),
                decoration: BoxDecoration(
                  borderRadius: BorderRadius.circular(10),
                  border: Border.all(color: Colors.blueAccent,width: 2)
                ),
                child: Text('Simple Border'),
              )` 


## Different radius to each corners

    `Container(
                padding: EdgeInsets.all(10),
                decoration: BoxDecoration(
                  borderRadius: BorderRadius.only(topLeft: Radius.circular(10),bottomRight: Radius.circular(10)),
                  border: Border.all(color: Colors.blueAccent,width: 2)
                ),
                child: Text('Simple Border'),
              )` 

## Conclusion

I hope you get an idea how to add border and customize that based on your need.
