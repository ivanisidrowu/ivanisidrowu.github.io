---
layout: post
title:  "Kotlin Flow Migration and Testing"
date:   2020-08-06 00:00:00
author: Ivan
categories: 
- Kotlin
tags: Kotlin
---

Kotlin Flow is an asynchronous stream open source library based on top of Kotlin Coroutines. There are different ways to test code written by Flow. In this article, we will discuss the methods of testing it and pros & cons. Since Flow shares similarities with Rx streams, I will also share things about the comparison between Flow and Rx. Additionally, I will show you some examples of migrating from Rx to Flow. At last, we will share the experience of using Flow in production.

# RxJava VS. Flow
Both of them can handle asynchronous streams, and they are often compared with each other. Let's see their comparisons.
## Comparison
* Learning curve
  
  Flow is simple and easy to learn because it only has one stream type. Comparing to Flow, RxJava has 5 stream types like Observable, Flowable, Single and more. For people who haven't used reactive streams before, it might be easier to understand and implement one type instead of many of them.

* Kotlin language features supports
  
  Flow has Kotlin language feature support since it is from Kotlin. Unlike RxJava, if you want to use Kotlin language feature on RxJava, you will have to import RxKotlin or implement it by yourself.

* Operators
  
  Flow has less operators than RxJava. This could be a pro and also a con. On one side, you don't need to check out the documents so frequently since there are not so many operators. On the other side, if you have a complicated process you will need to customize your own operators by writing extension methods.

## Stream Types
### RxJava
Observable
* Flowable - Same as Observable, but with backpressure support.
* Single - 
* Maybe
  
  It might complete without providing any value.
* Completable
Without emitting values.
Flow
Flow, Flow, and Flow.
