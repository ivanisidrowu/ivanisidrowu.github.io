---
layout: post
title:  "Android Developers Learn Swift Part 1"
date:   2020-08-29 00:00:00
author: Ivan
categories: 
- Swift
tags: 
- Swift
- iOS
---

I've developed Android Apps for more than 6 years. The reason I wanted to learn Swift and iOS development is that I wanted to know more about the iOS ecosystem. Since my wife also wants to learn iOS, I think it's not a bad idea we can learn it together. I will write this series of articles in Android developers' view. In this article, it contains topics as below.

* Hello World (super legend classic thing of every language)
* Variables
* Conditionals
* Loops

## Hello World

First, let's see how they say "Hello World" in Swift.

```swift
print("Hello World")
```

And the Kotlin one...

```kotlin
print("Hello World")
```

Hmmm, they are the same, maybe they are brothers/sisters.
But what if I want to print some variables?

Swift:
```swift
let world = "World"
print("Hello \(world)")
```

Kotlin:
```kotlin
val world = "World"
print("Hello $world")
```

Swift also has `println`, you can use this one to print a line. An interesting thing is the place holder of string. We can use `\(yourVariableHere)` to represent the variable we want it to be in the string. Kotlin has the same thing, but all you have to type is `$yourVariableHere`.

For the comments...

```swift
// Single comment
/*
Multiline
*/
```

```kotlin
// Single comment
/*
Multiline
*/
```
Well, no difference at all.

## Variables

In Swift, it has variables and constants. Variables refer to a storage location in memory. The variable is mutable, so we can manipulate and save data by it. We can create a variable by `var`. On the other hand, constants stand for immutable values. We can use it to store values that are not changeable. `let` is the way it represents constants.

```swift
var mutable = "I'm changeable!"
let immutable = 0
```

Kotlin has the same idea. It uses `var` and `val` to represent variables and constants. However, for Java developers, the idea of immutable objects is not so clear in Java. If we want to represent constants, we might consider to mark it as static values or objects.

```kotlin
var mutable = "I'm changeable too!"
val immutable = 0
```

## Conditionals

Swift
```swift
if hungry {
    eat()
} else {
    dontEat()
}
```

Kotlin
```kotlin
if (hungry) {
    eat()
} else {
    dontEat()
}
```

When I was practicing conditionals in Swift, the thing I don't get used to is not to write parentheses. Even for `switch`, it does not need to write parentheses. Isn't it great?

Swift
```swift
var color = "green"
switch color {
    case "red":
        print("It's red.")
    case "green":
        print("It's green.")
    case "yellow":
        print("It's yellow.")
    default:
        print("Unknown.")
}
```

`switch` is not enough, Swift has another interesting statement, called "where clause".

```swift
let num = 3

switch num {
  case let x where x % 2 == 0:
    print("\(num) is even")
  case let x where x % 2 == 1:
    print("\(num) is odd")
  default:
    print("\(num) is invalid")
}
```

We can consider `let x` is a temporary variable to the condition after the let expression. So we can use `x` to write the condition inside every case. In Kotlin, it also has a similar feature, but the expression is a little different. Instead of using `switch`, Kotlin has `when` statement. In my opinion, Kotlin's `when` is more concise.

Kotlin
```kotlin
val x = 3
    
when {
    x % 2 == 0 -> {
        print("$x is even")
    }
    x % 2 == 1 -> {
        print("$x is odd")
    }
    else -> {
        print("I don't know what it is.")
    }
}
```

Logical operators in Swift are the same as other languages except for one thing, `&&` has higher priority than others. For example,

```swift
false || true && false   // false
```

So it's actually can be re-written to this one.

```swift
false || (true && false)
```

## Loops

Swift has the expression of ranges.

```swift
let ranges = 1...3
for x in ranges {
    println(x)
}
// It prints 1 to 3.
```

But what if we want to print ranges with step counts? Well, we can use `stride` function!
It has 3 parameters, `from`, `to`, and `by`. `by` means how to increment/decrement from the start to the end.

```swift
for oddNum in stride(from: 1, to: 5, by: 2) {
  print(oddNum)
}

// Prints: 1
// Prints: 3
```

Not every case needs a variable in loops, underscore could be useful when we don't need a placeholder variable.

```swift
for _ in 1...3 {
    print("Yea!")
}
```