---
layout: post
title:  "Kotlin Flow Migration and Testing"
date:   2020-08-06 00:00:00
author: Ivan
categories: 
- Kotlin
tags: 
- Kotlin
- Android
---

It's been almost two years or so since I wrote my last blog article. I got a lot of things to share recently, no matter it's related to work or not. I think it's better for me to record and share them, so I started writing again. I'm not a native English speaker, if I wrote something doesn't make sense, please let me know. ：)

Kotlin Flow is an asynchronous stream open source library based on top of Kotlin Coroutines. There are different ways to test code written by Flow. In this article, we will discuss the methods of testing it and its pros & cons. Since Flow shares similarities with Rx streams, I will also share things about the comparison between Flow and Rx. Additionally, I will show you some examples of migrating from Rx to Flow. At last, I will make a conclusion and share the experience of using Flow in production.

## RxJava VS Flow
Before we jump into the topic of migration and testing, let's see some comparisons between Flow and Rx, so we can understand how Flow works.

## 1. Comparison
* Learning curve
  
  Flow is simple and easy to learn because it only has one stream type. Comparing to Flow, RxJava has 5 stream types like Observable, Flowable, Single, and more. For people who haven't used reactive streams before, it might be easier to understand and implement one type instead of many of them.

* Kotlin language feature support
  
  Flow has Kotlin language feature support since it is from Kotlin. Unlike RxJava, if you want to use Kotlin language feature on RxJava, you will have to import RxKotlin or implement it by yourself.

* Operators
  
  Flow has fewer operators than RxJava. This could be a pro and also a con. On one side, you don't need to check out the documents so frequently since there are not so many operators. On the other side, if you have a complicated process you will need to customize your own operators by writing extension methods.

## 2. Stream Types
### RxJava
* Observable - The most common one.
* Flowable - Same as Observable, but with backpressure support.
* Single - Emits single value.
* Maybe - It might complete without providing any value.
* Completable - Emits no values.
### Flow
Just Flow, period. 😁

## 3. Examples
Let me show you some simple examples to see their differences.

#### RxJava
```kotlin
Observable
    .just("Pikachu", "John", "Blabla")
    .subscribe { name ->
        println(name)
    }

```
#### Flow
```kotlin
flowOf("Pikachu", "John", "Blabla")
    .collect { name ->
        println(name)
    }
```
As you can see, their expression are similar. For RxJava, we use `Observable.just(...)`, and for Flow, we use `flowOf(...)`. They are pretty much the same thing.

#### RxJava
```kotlin
Observable
        .just("Pikachu", "John", "Blabla")
        .map { names -> names[5] }
        .subscribe(
            { ch -> println(ch) },
            { e -> println(e) },
            { println("Completed") }
        )
```
If we want to handle errors and completion, in RxJava, we need to add two callbacks into `subscribe()` function. However, you can also use [RxKotlin](https://github.com/ReactiveX/RxKotlin) to simplify the code.

```kotlin
Observable
        .just("Pikachu", "John", "Blabla")
        .map { names -> names[5] }
        .subscribeBy(
            onSuccess = { ch -> println(ch) },
            onError = { e -> println(e) },
            onComplete = { println("Completed") }
        )
```

Isn't it better than the previous one? 

#### Flow
```kotlin
flowOf("Pikachu", "John", "Blabla")
    .map { names -> names[5] }
    .catch { e -> println(e) }
    .onCompletion { println("Completed") }
    .collect { ch -> println(ch) }
```

As for Flow, we don't need to import another extensions to support kotlin language features. We can simply add separate lambdas into the Flow. The `catch()` function is not the only way to handle Flow errors. You can also try/catch Flow, but I don't recommend you to do it.


#### RxJava
```kotlin
Single.just(6)
    .subscribe { number ->
        println(number)
    }
```
RxJava uses `Single` to emit single value.

#### Flow
```kotlin
flow { emit(6) }
    .collect { number ->
        println(number)
    }
```

In Flow, we can emit single value inside of the `flow` lambda to acheive the same effect as RxJava `Single`.

## 4. Theading

#### RxJava Schedulers
* `io`
* `computation`
* `mainThread`
#### Flow Dispatchers
* `IO`
* `Default`
* `Main`

Except their names are different, the function of them are similar.

### Threading Operators

#### RxJava
* `subscribeOn`
* `observeOn`
  
#### Flow
* `flowOn`

They switch threads by using different strategy. Let me show you some examples.

```kotlin
observeSomething()
    .subscribeOn(io())
    .observe(mainThread())
    .subscribeOn(computation())
    .subscribe { result -> println(result) }
```
In RxJava, we declare start and modify chain below. So the second `subscribeOn` will be ignored. The code under `subscribeOn(io())` will be executed in IO thread.

```kotlin
CouroutineScope(Job() + Dispatchers.Main).launch {
    doSomethingInFlow()
    .flowOn(Dispathcers.IO)
    .map { result -> result.length }
    .flowOn(Dispatchers.Default)
    .collect { result -> println(result) }
}
```
On the other hand, in Flow, we declare and modify chain above. You have to be very careful with `flowOn` expression. See the above example, we can separate this code into 3 parts. First, `doSomethingInFlow()` runs on IO thread. Second, `map` runs on `Default`. Lastly, the final `collect` runs on `Main` which depends on the outer coroutine scope. Quick summary is they work in opposite. So when we are using Flow, we need to understand how `flowOn` threading works first.

## 5. Migration from RxJava to Flow

For RxJava users who want to migrate to Flow. There is no need to do the migration all at once. The reason is that your commits might be huge and different to review. It's for the best that we do it step by step. Lucky for us, Kotlin provides a library can help us do the migration, called [kotlinx-coroutines-rx2](https://github.com/Kotlin/kotlinx.coroutines/tree/master/reactive/kotlinx-coroutines-rx2). It has observables transformation, suspending extensions, and suspending iteration. The usage of this library is actually simple.

### Observable to Flow

```kotlin
runBlocking {
    Observable.just(1, 2, 3, 4, 5)
            .asFlow()
            .flowOn(Dispatchers.IO)
            .collect { }
}
```

### Flow to Observable

```kotlin
flowOf(1, 2, 3, 4, 5)
    .asObservable()
    .subscribeOn(Schedulers.io())
    .observeOn(mainThread())
    .subscribe()
```

### Single to Flow

```kotlin
runBlocking {
    flow { emit(getSingle().await()) }
}
```
We can use flow to emit the single value from the RxJava's `Single`.

## 6. Testing

There are two ways to test Kotlin Flow as I know. The first one is to use mocking libraries. Just like how we normally test our code. To maximize the usage of Kotlin language features, I suggest that we use mocking libraries which is specifically targeting Kotlin code, such as [mockito-kotlin](https://github.com/nhaarman/mockito-kotlin) and [MockK](https://mockk.io/). These two libraries can test code well, but I recommend MockK because it has DSL-like syntax and a lot of extremely useful functions and extensions. The examples below show you how to test Flow by using MockK.

### Using MockK
```kotlin
data class User(val name: String, val id: Long)

interface Service {
    fun getUser(): User
}

class ApiService : Service {
    override fun getUser() = User("John", 1L)
}

class UserRepository(private val service: Service) {
    fun getUser(): Flow<User> = flow {
        emit(service.getUser())
    }
}
``` 

Our goal is to test the `UserRepository`. We can write code as below...

```kotlin
@Test
fun `Test getting an user info by MockK`() = runBlocking {
    val fakeUser = User("Doe", 2L)

    val mockApiService = mockk<ApiService>(relaxed = true)
    every { mockApiService.getUser() } returns fakeUser

    val userRepository = UserRepository(mockApiService)
    userRepository.getUser().collect {
        it shouldBe fakeUser
    }
}

infix fun Any?.shouldBe(expect: Any?) {
    Assert.assertEquals(expect, this)
}
```

### Using Flow Assert

Another way to test Flow is to use Flow Assert. I saw this one on blogs, and I think this is quite interesting to test code in a different way. It's origially from a library, called [SqlDelight](https://github.com/cashapp/sqldelight/blob/master/extensions/coroutines-extensions/src/commonTest/kotlin/com/squareup/sqldelight/runtime/coroutines/FlowAssert.kt#L28). People found out that they took advantage of `Channels` and `Coroutines` API to test Flow. Using Flow Assert makes expressions more specific. It provides 3 functions to help us assert the testing result.

* `ExpectedItem()`: Returns an expected item.
* `ExpectedError()`: Returns an expected error.
* `ExpectedComplete()`: Asserts the completion of the targeted Flow.

I know it sounds like Flow Assert might be a complicated thing but it is just an extension method.

```kotlin
Flow<T>.test(timeout: Long, validate: suspend FlowAssert<T>.() -> Unit) {
    ...
}
```

The receiver type of the function provides `FlowAssert`'s member for use in lambda. As we mentioned, under the hood, it uses `Channels`. The source code of the extension does works like this... Suppose that we have a `Flow` emits items to the unlimited buffer `Channel` to queue the items. Whenver we call `Flow<T>.test(...)`, `FlowAssert` will query the channel and assert the items for us. If you want to learn more about the concept of `FlowAssert`, you can read [this blog](https://proandroiddev.com/kotlin-flow-assert-ff45465c01c0) for more information. Let's see some examples!

```kotlin
@Test
fun `Test getting an user info by Flow Assert`() = runBlocking {
    val fakeUser = User("Pikachu", 3L)
    val fakeApiService = provideFakeService(fakeUser)

    val userRepository = UserRepository(fakeApiService)
    userRepository.getUser().test {
        expectItem() shouldBe fakeUser
        expectComplete()
    }
}
```

```kotlin
@Test
fun `Test getting an error by Flow Assert`() = runBlocking {
    val fakeApiService = provideFakeServiceThrowsException()

    val userRepository = UserRepository(fakeApiService)
    userRepository.getUser().test {
        expectError() is IOException
    }
}
```

## 7. Conclusion

Kotlin Flow gives us flexible structure to handle reactive streams. Although it doesn't provide a lot of operators, the operators it has is actually enough for use to deal with streams. Additionally, writing custom operators for Flow is really simple. We just write the extension of Flow. Recently I use Flow on production, and there is no issues so far. I mainly use Flow with Retrofit. All we need to do is to write a adapter and a factory to transform Retrofit calls to Flow. You have to be careful when you are retrying the requests. Remember, don't use the same Retrofit call, it will crash your App.