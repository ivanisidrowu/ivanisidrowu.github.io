---
layout: post
title: 'ProGuard + Android: To shrink, optimize, and obfuscate codes'
date: '2013-11-28T08:39:00.002-08:00'
author: Ivan
tags:
- Android
categories:
- Android
modified_time: '2014-03-28T00:31:32.566-07:00'
thumbnail: https://lh6.googleusercontent.com/cQX3C_c-riNLk0YPJdg_azwK3J-yBhxODQPoOcsfVPiDQZpwc7rwm_CyKOJq34s-nOYVd41Bheewjl94rBqVGywsCNwP9a8Mqy9rn6F1RZFwZmZcagNXca7rrV7D=s72-c
blogger_id: tag:blogger.com,1999:blog-415098333644749081.post-439353595896787870
blogger_orig_url: http://invictuscode.blogspot.com/2013/11/proguard-android-to-shrink-optimize-and.html
---

Recently, I have to survey ProGuard. We are planning to use ProGuard to protect codes of Android application. As this title, ProGuard can also shrink and optimize APP. So it's quite useful for Android developers. Android Developers website doesn't have much information of ProGuard. I suggest everyone go to ProGuard website for more information.

  

See [http://developer.android.com/tools/help/proguard.html](http://developer.android.com/tools/help/proguard.html) 

And this one has more information... [http://proguard.sourceforge.net/](http://proguard.sourceforge.net/)

  

Besides ProGuard, DexGuard is another choice.  
It seems designed for Android Apps. However, if you wanna use it, you need to buy license.  
  
To enable ProGuard, we can modify a file in Android project, "project.property".  
  
Find this line:  
`#proguard.config=${sdk.dir}/tools/proguard/proguard-android.txt:proguard-project.txt`  
Then remove the symbol "#" to enable ProGuard tools.  
![](https://lh6.googleusercontent.com/cQX3C_c-riNLk0YPJdg_azwK3J-yBhxODQPoOcsfVPiDQZpwc7rwm_CyKOJq34s-nOYVd41Bheewjl94rBqVGywsCNwP9a8Mqy9rn6F1RZFwZmZcagNXca7rrV7D)  
  
After finishing property modification, you can export  APK by doing this:  
Right-click on your project -> select "Android Tools" -> Export Signed/Unsigned package  
  
This is a way to use ProGuard on Android application by using default configuration in ${sdk.dir}/tools/proguard/proguard-android.txt:proguard-project.txt  
  
![](https://lh4.googleusercontent.com/pn8rMjwN9tUZiEatkYqOvodqQqn632HCUt8-jZrVcIHtmuU4HLD5n7pNDJtU1uQfRG6Ql0RByibLKSY95lJFHwR6AVhFgcxU5uPHzA0wAVq2WLJndCbVNsKOrJ-x)  
After exporting APK, you will see four txt files had been added: dump.txt, mapping.txt, seeds.txt, and usage.txt.  
  
If you want to have custom configuration, you can modify "<your\_project\_root>/proguard-project.txt".  
The configuration will override the default one.  
  
Reference this website: [http://proguard.sourceforge.net/#manual/usage.html](http://proguard.sourceforge.net/#manual/usage.html)  
There are many configuration options we could use.  

Most important option of all is "-keep". It helps preserving classes, methods, and fields.

It's essential to keep some classes on Android, such as classes extend Activity, Service, and so on.

This website also have an example of Android application. The example worth to take a quick look. It has useful configuration suggestions for adopting options. However, it's not that easy to write a perfect configuration when your app getting complicated. So you might need to spend some time and effort on it. I suggest beginners to read following sections: A complete Android application, Processing native methods, Processing native methods, Processing enumeration classes, and Processing serializable classes.

  

Let's take a look at the exported APK file.  

[![](http://4.bp.blogspot.com/-YM-QBr7Dg30/UpdzygdqVRI/AAAAAAAAALM/M4SaXCvlsw4/s1600/123123.png)](http://4.bp.blogspot.com/-YM-QBr7Dg30/UpdzygdqVRI/AAAAAAAAALM/M4SaXCvlsw4/s1600/123123.png)

  
The package and classes names had been obfuscated.  
So if someone crack your APK, he/she will get nothing!