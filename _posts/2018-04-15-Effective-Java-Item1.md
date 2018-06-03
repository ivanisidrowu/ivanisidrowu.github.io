---
layout: post
title:  "Effective Java Item 1: Consider Static Methods Instead of Constructors"
date:   2018-04-15 10:00:00
author: Ivan
categories: Effective Java
---
This is the first article about Effective Java Items. I read the 2nd version not the 3rd version which is just came out about a month ago. Item 1 is talking about static factory methods. This pattern is common in Android development. For example, when we are using Toast, we pass parameters and the static methods will return a Toast instance for us depending on the parameters. Using static factory methods sometimes could be good, but not always good because it has advantages and disadvantages.

# Static Factory Method
## Advantages
* Static factory methods have name unlike Constructors.

We can give a name to the static factory methods. When we are using them, the name of the methods will provide meaningful information for us to use static factory methods. Conversely, Constructors can not do that. We can only guess the meaning from the parameters. In addition, due to the overloading rules, we can only have one unique sequence of parameters in Constructors, so if you want create an object under different circumstances. Some developers temp to shift around the sequences of parameters, but this is not the right implementation.

* Static factory methods are not required to create a new obejct each time they are invoked.

If creating an instance of a class is expensive, we could cache an instance then return it. So we don't have to create it each time we invoke the method. For example, in Android world, we often use singleton pattern to return the same instance and to be sure that we only create it once.

* Static factory methods are able to return any subtype of the return type.

* Static factory methods reduce the verbosity of creating parameterized type instances.

In Android, sometimes we want to launch the same activity or fragments with the same intents and bundles. Writing those paramters and setting them into intents are boilerplates. Static factory methods can be useful in this case.

```kotlin
class ExampleActiviy: AppCompatActivity() {
  companion object {
    fun newIntent(): Intent {
      // ...
    }
  }
}
```

## Disadvantages
* If you provide only static factory methods and the return classes do not have public or protected constructors, they cannot be subclassed.

* Sometimes, static factory methods are not readily distinguishable from other static methods.

Since it's a method, sometimes it's hard for the user of your API to know which static method to use. Even more, they might not know there are static methods. This situation can be solved by using proper conventions.

This series of articles are notes when I am reading a book, called Effective Java (2nd version). If you are interested, you should definitely check it out.
