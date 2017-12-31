---
layout: post
title:  "Try Kotlin"
date:   2017-07-02 14:37:44
author: Ivan
categories: Android
---
Since [Google I/O 2017](https://www.youtube.com/watch?v=X1RVYt2QKQE&t=682s), I think I should learn Kotlin because they announced that Kotlin has became one of the offical support languages in Android. So I want to introduce some of Kotlin features. Also, it's for me to recap how I learn Kotlin in short.

## Data Types
One of the greatest things of Kotlin is that the data types in it are easier to understand than Java in my personal opinion. In Kotlin, you can describe data types in two ways, var and val. What's the difference between them? Var is chanable variable, but val is not. It's a value. It's kind of like in Java, when you give a variable final modifier. You can find more information [here](https://kotlinlang.org/docs/reference/basic-types.html).

```kotlin
var num = 9
val listSize = 10
```

In addition, you can also specify variable type to a variable. For example, like the code below, the num variable is specified as Integer type. What's more, in Kotlin, it has basic methods and functions in objects. In this example, num2 can be transformed to Integer easily.

```kotlin
var num: Integer = 9
var num2: Double = 1.222
println(num2.toInt())
println(88.toChar())
```

## Strings
Strings processing might be a little bit easier to deal with in Kotlin because it has basic methods to deal with normal cases.

```kotlin
var str = "John is a boy."
println("index 2 to 7 ${str.subSequence(2,7)}")

println(" the 2nd one ${str[1]}")
```
## Array
Well, let's look at the code, no need to describe more.

```kotlin
// new an array
var arr = arrayOf(2, "soya", 1.2)

// contains soya?
println("is contain soya = ${arr.contatins("soya")}")

// take the first one
var first = arr.first()

// index search
val index = arr.indexOf(2)

// you can also put functions inside arrays
var funArray = Array(1, 3, {x->x*x}, {x->x+x})

println(funArray[2])
println(funArray[3])
// then the printed value will be 4 and 6
```
## Ranges
Kotlin provides [.. operator](https://kotlinlang.org/docs/reference/ranges.html) form helps to experss ranges.

```kotlin
val valueTen = 1..10
val charsAToZ = "A".."Z"
val tenToZero = 10.downTo(0)
val threeToThirty = 3.rangeTo(30)
```
## Conditionals
In Kotlin, [most of the coniditionals](https://kotlinlang.org/docs/reference/control-flow.html) are just like the ones in Java. So I'm going to introduce something that is not like Java. For instance, when id is in 0 to 5, it prints part one. If it's 6, it prints out part two. Also if it is 7, 8, or 9, part three will be printed. Anything else will print part four.

```kotlin
when(id) {
  in 0..5 -> println("part one")
  6 -> println("part two")
  7,8,9 -> println("part three")
  else -> println("part four")
}
```
## Funtions
In the example below, getSum function takes varargs as many integers and then return an integer. Functions is also easy to understand if you have learned other languages as well.

```kotlin
fun main(args: Array<String>) {
  println("Sum = ${getSum(1,2,3)}")
}

fun getSum(varargs nums: Int): Int {
  var sum = 0
  nums.forEach {n -> sum += n}
  return sum
}
```
## Higher Order Functions
In short, [higher order function](https://kotlinlang.org/docs/reference/lambdas.html) is a function that takes or returns a function. It's not easy to understand if you've only learned JAVA but not other languages like javascript and so on.
```kotlin
val nums = 1..10
val evenNums = nums.filter {it & 2 == 0}
eventNums.forEach{ n -> println(n) }
```
It's worth to mention that Kotlin has the operators like Java 8 or RxJava that can let you do some practical stuff to collections, for example, filter, map, reduce, and so on. If you are interested in that, you can check the official documents for more information. (the code above show you exactly how to use filter operator)

## Extension Functions
Another useful feature will be [extension functions](https://kotlinlang.org/docs/reference/extensions.html). Just like C#, Kotlin also have extension functions. Let's take a look at examples from document.

```kotlin
fun MutableList<Int>.swap(index1: Int, index2: Int) {
    val tmp = this[index1] // 'this' corresponds to the list
    this[index1] = this[index2]
    this[index2] = tmp
}

val l = mutableListOf(1, 2, 3)
l.swap(0, 2) // 'this' inside 'swap()' will hold the value of 'l'
```
I don't need to actually extend MutableList class, and I can add a function to it. For me, it's really cool!

## Null Checks
In Java, null checks could be really annoying. Imagine that you have to check null before you do everything and there is no other elegant way to do it, and it sucks. In Kotlin, you can use [? operator](https://kotlinlang.org/docs/reference/null-safety.html) to do null check, and also they have other operators to help you deal with other situations. The operators could be !!, elvis operator, so on so forth.

### Java
```java
if(activity != null) {
  if (mView != null) {
    mImage = (ImageView) activity.findViewById(R.id.image);
    mViewCount ++;
  } else {
    mViewCount --;
  }
}
```
### Kotlin
```kotlin
if(mView != null) {
  mImageView = activity?.findViewById(R.id.image)
  mViewCount ++;
} else {
  mViewCount --;
}
```
## Summary
Kotlin seems really charming to me because I like its expressions and operators. Many features let me want to put down my JAVA code and go to use Kotlin immediately although now I just new to Kotlin, I still have a lot to learn. I found some of my examples in [this video](https://www.youtube.com/watch?v=H_oGi8uuDpA) which is really helpful for people who wanna get started to work on Kotlin. I highlyu recommend! I really excited that Android studio will support Kotlin in 3.0 version. It will be a game chaning version for android development!
