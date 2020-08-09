---
layout: post
title: Code Execution Time on Android
date: '2014-03-27T22:47:00.001-07:00'
author: Ivan
tags:
- Android
categories:
- Android
modified_time: '2014-03-28T00:31:17.104-07:00'
blogger_id: tag:blogger.com,1999:blog-415098333644749081.post-4325110101444751448
blogger_orig_url: http://invictuscode.blogspot.com/2014/03/code-execution-time-on-android.html
---

When it comes to improving performance, code execution time becomes really important.  
Is there any simple way to get code execution time on Android?  
The answer is YES!  
I didn't know this before doing some research online...  
All you have to do is just new "TimingLogger"!  
  

TimingLogger mTimingLogger = new TimingLogger(YOUR\_TAG, "theMethod");  
 // Code Part1  
mTimingLogger.addSplit("CodePart1");  
// Code Part2  
mTimingLogger.addSplit("CodePart2");  
mTimingLogger.dumpToLog();

  
The execution time of code part 1 and part 2 will be printed on adb logcat.  
However, before you print it, you have to set log level to VERBOSE.  
So here is an example...  
First of all, open your cmd command line, and cd to /platform-tool/ Then, type the following commands to set log level to VERBOSE.  

adb shell setprop log.tag.plzTypeYourTagHere VERBOSE

After finishing all steps above, you can run your application. Your log printing will be like this one...  

03-28 13:41:23.695: D/ivan(18099): theMethod: begin  
03-28 13:41:23.695: D/ivan(18099): theMethod:      1 ms, codePart1  
03-28 13:41:23.695: D/ivan(18099): theMethod:      3 ms, codePart2  
03-28 13:41:23.695: D/ivan(18099): theMethod: end, 4 ms

Use this method can let us know code execution time and try to improve the performance.  

BTW, if you are interested in how I print code on Blogger, you can reference this article... [http://www.stylifyyourblog.com/2012/07/syntax-highlighting-in-blogger-using.html](http://www.stylifyyourblog.com/2012/07/syntax-highlighting-in-blogger-using.html)