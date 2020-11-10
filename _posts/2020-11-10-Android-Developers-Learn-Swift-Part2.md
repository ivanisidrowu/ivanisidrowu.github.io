---
layout: post
title:  "Android Developers Learn Swift Part 2"
date:   2020-11-10 00:00:00
author: Ivan
categories: 
- Swift
tags: 
- Swift
- iOS
---

This is the second part of the "Android Developers Learn Swift" articles. The topics include:

* Array
* Sets
* Dictionaries
* Functions


If you haven't read part 1, you can read it [here](https://ivanisidrowu.github.io/swift/2020/08/29/Android-Developers-Learn-Swift-Part1.html)! :)

## Array

Let's get started with a Swift example!

Swift:
```swift
var arr: [Int] = [1,2,3]
```
Kotlin:
```kotlin
var arr: IntArray = intArrayOf(1, 2, 3)
```

At first glance, you might think Swift is more concise in this case. However, there are more ways to declare an integer array in Kotlin. Also in Swift, we can declare it in many different ways.

Swift:
```swift
var arr = [1, 2, 3]
```
Kotlin:
```kotlin
var arr = arrayOf(1,2,3)
```

As you can see, both of them can figure out type automatically. We don't have to specify types for the variables.

Array manipulations are basically the same, such as position index, getting elements from the array, and so on.
In some cases, operations are the same, but the function names are different...

Swift:
```swfit
var arr = [4, 5, 6]
print(arr.count) // prints 3
```

Kotlin:
```kotlin
var arr = arrayOf(4, 5, 6)
print(arr.size) // prints 3
```

There is a huge difference when using Swift Array. In Swift, Array could be dynamic. What's that mean? It means the array can insert and change its size dynamically. However, in Kotlin, Array can not do that. Instead, we can use ArrayList in Kotlin. So let's see some comparison between Swift Array and Kotlin ArrayList.

Swift:
```swift
var animalArray = ["cat", "dog"]
animalArray.append("tiger")
animalArray += ["bird"]
```

Kotlin:
```kotlin
var animalArray = arrayListOf("cat", "dog")
animalArray.add("tiger")
animalArray.add("bird")
```

Swift has a cool operator, `+=`, to append elements to the existing array. Kotlin does not have that.

## Sets

Swift:
```swift
var names: Set = ["Sam", "John", "Mike"]
var emptyNames = Set<String>()
```

Kotlin:
```kotlin
var names = mutableSetOf("Sam", "John", "Mike")
var emptyNames = mutableSetOf<String>()
```

In Kotlin, we can choose which kind of set we would like to use such as `LinkedHashSet` and `HashSet`, but in Swift, there is no set like `LinkedHashSet`. We also can make the set immutable or mutable by specifying `setOf` or `mutableSetOf` depends on your requirements.

Ｗhat if we want to do set intersection?
No problem, they have the feature!

Swift:
```swift
var setA: Set = ["A", "B", "C", "D"]
var setB: Set = ["C", "D", "E", "F"]
 
var setC = setA.intersection(setB) 
// ["D", "C"]

var setD = setA.union(setB)
// ["B", "A", "D", "F", "C", "E"]

var setE = setA.symmetricDifference(setB) 
// ["B", "E", "F", "A"]
 
var setF = setA.subtracting(setB)
// ["B", "A"]
```

Kotlin:
```kotlin
val setA = setOf("A", "B", "C", "D")
val setB = setOf("C", "D", "E", "F")
val setC = setA intersect setB
val setD = setA union setB
val setF = setA subtract setB
```

They all have `union`, `intersection`, and `subtract`. Unfortunately, Koltlin does not have `symmetricDifference`, but Kotlin can use Java API to do it. (`Sets.symmetricDifference`) 

## Dictionary

They call Dictionary in Swift, we call `Map` in Kotlin.

Swift:
```swift
var dictionaryName = [
  "Key1": "Value1",
  "Key2": "Value2",
  "Key3": "Value3"
]
```

Kotlin:
```kotlin
val dictionaryName = mutableMapOf(
    "Key1" to "Value1",
    "Key2" to "Value2",
    "Key3" to "Value3"
)
```

There is a difference, Kotlin can specify the map type, `LinkedHashMap` or `HashMap`, just like we mentioned earlier. `mutableMapOf` declares a mutable map. If you want to have an immutable map, you can just use `mapOf`.

Adding, updating, and removing key-value pairs are similar. Check out the example below.

Swift:
```swift
dictionaryName["Key4"] = "Value4"
dictionaryName["Key1"] = "UpdatedValue1"
dictionaryName.updateValue("UpdatedValue1", forKey: "Dime")
dictionaryName.removeValue(forKey: "Key3")
```

Kotlin:
```kotlin
dictionaryName["Key4"] = "Value4"
dictionaryName["Key1"] = "Value1"
dictionaryName.remove("Key3")
```

Comparing to Swift, Kotlin does not have `updateValue` function, but I think Kotlin's expression is more concise than Swift in this case.

## Functions

Swift:
```swift
func printName(name: String) -> Void {
    print(name)
}

func getAge() -> Int {
    return 20
}
```

Swift functions can be declared by `func` keyword. At the end of the `func`, we can specify the return value. For example, in the function `printName`, it takes `name` as a parameter and returns nothing. On the other hand, the function `getAge`, it returns an integer. Let's look at Kotlin code.

Kotlin:
```kotlin
fun printName(name: String) {
    print(name)
}

fun getAge() = 20
```

The return type of the function in Kotlin can be declared at the end of the `fun`. So we can also rewrite `getAge` function as `fun getAge(): Int = 20`. Kotlin has a sweet expression ♥️, you can return a value by a single `=` sign.

Swift has a power that Kotlin doesn't have. Swift can return multiple values without wrapping them into a class or structure.

Swift:
```swift
func getPerson() -> (name: String, age: Int, lastName: String) {
    return ("John", 18, "Mayer")
}
```

The return values will be converted into a tuple automatically. On the other hand, if we rewrite this code in Kotlin.

Kotlin:
```kotlin
fun getPerson(): Triple<String, Int, String> {
    return Triple("John", 18, "Mayer")
}
// or wrap it into a data class
fun getPerson() = Person("John", 18, "Mayer")

data class Person(val name: String, val age: Int, val lastName: String)
```

Hmm... I like the expression of Swift in this case, it's quite simple and understandable.

Swift and Kotlin, they both have default parameters.

Swift:
```swift
func printName(name: String = "John") -> Void {
    print(name)
}
```

Kotlin:
```kotlin
fun printName(name: String = "John") {
    print(name)
}
```

For the variadic parameters, they both have their ways to declare it.

Swift:
```swift
func printCount(names: String...) -> Void {
    print(names.count)
}
```

Kotlin:
```kotlin
fun printCount(vararg names: String) {
    print(names.size)
}
```

Swift has in-out parameters that Kotlin doesn't have. The in-out parameters allow a function to reassign the value.

```swift

var currentName = "unchanged"

func changeName(name: inout String) {
    name = "changed"
}

print(currentName) // unchanged
changeName(&currentName)
print(currentName) // changed
```

It's very different from an Android developer's point of view. Because in Java or Kotlin, we don't need to care what kind of the parameter is, but in this way, when we are writing code, we can immediately notice that we are using a changeable variable. In my opinion, it's an advantage to secure the code.

---

We saw some differences between Swift and Kotlin. They have their pros & cons. I think we should not compare them just like we are trying to decide which one is the champion of programming language. We can look at them from another perspective, to discover their beauties. There is no champion! In my next article, I will write about structures and classes. :)