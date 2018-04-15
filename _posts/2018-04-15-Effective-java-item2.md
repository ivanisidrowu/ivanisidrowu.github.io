---
layout: post
title:  "Effective Java Item 2: Consider a Builder When Face with Many Constructor Parameters"
date:   2018-04-15 10:00:00
author: Ivan
categories: Effective Java
---
Builder pattern is a really common pattern when we try to create an instance that has many parameters. For example, if we want to create a bean and the bean has a lot of required and optional parameters. Using builder to solve this problem can be really useful.

## Advantages
* The builder pattern is a good choice when designing classes whose constructors or static factories would have more than a handful of parameters.

It's just like Android AlertDialog. We can decide which parameters in the dialog by using builder.
```java
AlertDialog.Builder builder = new AlertDialog.Builder(getActivity());
builder.setMessage(R.string.dialog_fire_missiles)
         .setPositiveButton(R.string.fire, new DialogInterface.OnClickListener() {
             public void onClick(DialogInterface dialog, int id) {
                 // FIRE ZE MISSILES!
             }
         })
         .setNegativeButton(R.string.cancel, new DialogInterface.OnClickListener() {
             public void onClick(DialogInterface dialog, int id) {
                 // User cancelled the dialog
             }
         });
// Create the AlertDialog object and return it
builder.create();
```
## Disadvantages
* Write more code

You have to write another builder class. Another pointer is that it's harder to add parameters to the class which the builder create instance for you.
