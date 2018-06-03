---
layout: post
title:  "Effective Java Item 7"
date:   2018-06-03 14:00:00
author: Ivan
categories: Effective Java
---
## Effective Java Item 7: Avoid finalizer

* Finalizers are unpredictabile, often dangerous, and unnecessary.

* Never do anything time-critical in finalizers.

* Servere performance penalty for using finalizer, for example, time to create and destroy simple object goes from 5.6ns to 2400ns.

* Should not depend on finalizers to upgrade important persistence state.

* Provide specific termination method instead of writing code in finalizers.

* Finalizers should log if the resource has not been terminated.

* Anything throws exceptions in finalizers will be ingnored.

* It's better to take Advantages of Android lifecycler instead of using finalizers.

* Use finalizers as a safety net or to terminate noncritical native resources.

For instance...
```java
try {
   // get data from cursor
} catch (Exception e) {
   // exception handling
} finally {
  db.close();
}
```

I didn't have many chances to use finalizers, but as you can see, normally, we do not need it. The only one experience is that I write code about database transactions. Next time I see finalizers, I will have to check it very carefully.

This series of articles are notes when I am reading a book, called Effective Java (2nd version). If you are interested, you should definitely check it out.
