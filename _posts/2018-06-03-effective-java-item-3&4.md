---
layout: post
title:  "Effective Java Item 3&4"
date:   2018-06-03 00:00:00
author: Ivan
categories: 
- EffectiveJava
- Java
tags: Java
---

Enforce the singleton property with a private constructor or an enum type

### Advantages

* It's easy to be understood as a singleton class, and the class clients can get instance from getInstance method.
* If we change the APIs of the class, we don't need to modify much code.
* It uses private constructor to protect inner methods.
* It's more concise way to create singleton because it provides free serialization and singleton guarantees.

### Disadvantages

* Making a class a singleton makes it difficult to test its clients.


## Effective Java Item 4: Enforce noninstantiability with a private constructor

Since this one is a small item, I just listed down with few considerations.

* Private constructors prevent instantiation and subclassing.
* It's nonsense to ubstantiate util classes.

This series of articles are notes when I am reading a book, called Effective Java (2nd version). If you are interested, you should definitely check it out.
