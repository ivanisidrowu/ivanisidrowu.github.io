---
layout: post
title: Useful Android Studio Shortcuts and Tips
date: '2017-05-19T08:52:00.000-07:00'
author: Ivan
tags:
- Android
categories:
- Android
modified_time: '2017-05-19T08:52:34.957-07:00'
thumbnail: https://1.bp.blogspot.com/-o-ToY9xEBlQ/WR6pKiceY8I/AAAAAAAALwU/N7g6kLhmlcgpo36WzMZTPvpbKrv4B4p3QCK4B/s72-c/Navigate%2Bto%2Bnext%253Aprevious%2Btab.gif
blogger_id: tag:blogger.com,1999:blog-415098333644749081.post-2520489554831419612
blogger_orig_url: http://invictuscode.blogspot.com/2017/05/useful-android-studio-shortcuts-and-tips.html
---


I'm going to share some useful Android Studio shortcuts which help me a lot when I'm writing code. Some of the shortcuts you might know, but some of them I think most of developers don't even know them... I'm serious because when I knew live templates and postfix completion, I was like... what? I don't even know they have shortcuts this powerful. Taking advantages of shortcuts can help you write code faster and more efficient. The shortcuts in this post will not only contain general keyboard shortcuts but also live templates and postfix completions. I found these shortcuts really useful from the official [intellij docs](https://www.jetbrains.com/help/idea/2017.1/keyboard-shortcuts-and-mouse-reference.html) and an [Android Dev Summit 2015 video](https://www.youtube.com/watch?v=Y2GC6P5hPeA). So I will share shortcuts in three parts, general shortcuts, live templates, and postfix completions.

  
In this post, the keyboard shortcuts are for MAC users. If you are Windows or Linux users, in general you can replace CMD to Ctrl. Or you can checkout the preference in Android Studio. 

### General Shortcuts

*   Navigate to next/previous tab

Previous tab: CMD \+ Shift + \[   

Next tab: CMD + Shift + \]   
  

[![](https://1.bp.blogspot.com/-o-ToY9xEBlQ/WR6pKiceY8I/AAAAAAAALwU/N7g6kLhmlcgpo36WzMZTPvpbKrv4B4p3QCK4B/s640/Navigate%2Bto%2Bnext%253Aprevious%2Btab.gif)](http://1.bp.blogspot.com/-o-ToY9xEBlQ/WR6pKiceY8I/AAAAAAAALwU/N7g6kLhmlcgpo36WzMZTPvpbKrv4B4p3QCK4B/s1600/Navigate%2Bto%2Bnext%253Aprevious%2Btab.gif)

*   Navigate to next/previous edited location

Previous location: CMD + Shift + ←

Next location: CMD + Shift + →  

[![](https://2.bp.blogspot.com/-7nfoNSov4Z0/WR6pdi8P_oI/AAAAAAAALwc/Sw4SQBYhml0bQOSqPOPwE8dz2V3GDDVRQCK4B/s640/Navigate%2Bto%2Bnext%253Aprevious%2Bedited%2Blocation.gif)](http://2.bp.blogspot.com/-7nfoNSov4Z0/WR6pdi8P_oI/AAAAAAAALwc/Sw4SQBYhml0bQOSqPOPwE8dz2V3GDDVRQCK4B/s1600/Navigate%2Bto%2Bnext%253Aprevious%2Bedited%2Blocation.gif)  

*   Switcher

Alt + Tab  

[![](https://1.bp.blogspot.com/-T8CfTU35F7E/WR6tTzXVu6I/AAAAAAAALwo/s358qjfpQ-I9_SFz8s-qcR4DhB2Cp4IUwCK4B/s640/Switcher.gif)](http://1.bp.blogspot.com/-T8CfTU35F7E/WR6tTzXVu6I/AAAAAAAALwo/s358qjfpQ-I9_SFz8s-qcR4DhB2Cp4IUwCK4B/s1600/Switcher.gif)  

*   Switch to specific section

CMD + {Number} 

For example, CMD + 1 to project section. As you can see, on the left hand corner of the GIF below.

Respectively, CMD + 7 to the structure section, the tab which is under the project tab as we mentioned.  

[![](https://1.bp.blogspot.com/-N72k9fWJq0s/WR6t8teOiiI/AAAAAAAALw4/p-sNgODUYH0p7Hpn7eLC438eWsjqnt4qACK4B/s640/Switch%2Bto%2Bspecific%2Bsection.gif)](http://1.bp.blogspot.com/-N72k9fWJq0s/WR6t8teOiiI/AAAAAAAALw4/p-sNgODUYH0p7Hpn7eLC438eWsjqnt4qACK4B/s1600/Switch%2Bto%2Bspecific%2Bsection.gif)  

*   Maximize editor

CMD + Shift + F12

If you are using macbook, don't forget to press "fn" key together with the key combination.  

[![](https://1.bp.blogspot.com/-NbHccvy0tao/WR6uCroOj6I/AAAAAAAALxA/t2SKALPa96cWCzJodyOFP8xpl8Kv9U9zQCK4B/s640/Maximize%2Beditor.gif)](http://1.bp.blogspot.com/-NbHccvy0tao/WR6uCroOj6I/AAAAAAAALxA/t2SKALPa96cWCzJodyOFP8xpl8Kv9U9zQCK4B/s1600/Maximize%2Beditor.gif)  

*   Search everywhere

Double click Shift

This is really useful when you are searching something but you don't wanna just search for only one type of files or whatever, you can just use this to quickly locate the thing you are searching.  

[![](https://2.bp.blogspot.com/-kNHJ-imeMSI/WR6uJdbYhJI/AAAAAAAALxI/Y1mgaDWYDmYnBL8xHW8Io4TUPKBtwmHHgCK4B/s640/Search%2Beverywhere.gif)](http://2.bp.blogspot.com/-kNHJ-imeMSI/WR6uJdbYhJI/AAAAAAAALxI/Y1mgaDWYDmYnBL8xHW8Io4TUPKBtwmHHgCK4B/s1600/Search%2Beverywhere.gif)  

*   Show class structure

CMD + F12 (and don't forget fn if you use macbook)  

[![](https://1.bp.blogspot.com/-opzC33BftT8/WR6uLqTi-aI/AAAAAAAALxQ/FYz6fHoeHBwJY5SamDXPfZQ26ZI1iG67QCK4B/s640/Show%2Bclass%2Bstructure.gif)](http://1.bp.blogspot.com/-opzC33BftT8/WR6uLqTi-aI/AAAAAAAALxQ/FYz6fHoeHBwJY5SamDXPfZQ26ZI1iG67QCK4B/s1600/Show%2Bclass%2Bstructure.gif)  

*   Find actions

CMD + Shift + A

I highly recommend this shortcut, THIS IS EXETREMELY USEFUL! Why? Because you probably won't remember all shortcuts even for those commands doesn't have shortcuts. This shortcut can help us to use those commands without having to remember shortcuts. And to be honest, this one is my favorite shortcut. :D

[![](https://3.bp.blogspot.com/-ovZmOAVuN8w/WR6uyDfurjI/AAAAAAAALx8/A2rnByMiyfUfH5k0JiBIFSKW-S32PTA5QCK4B/s640/Find%2Bactions.gif)](http://3.bp.blogspot.com/-ovZmOAVuN8w/WR6uyDfurjI/AAAAAAAALx8/A2rnByMiyfUfH5k0JiBIFSKW-S32PTA5QCK4B/s1600/Find%2Bactions.gif)  

*   Show usages

Alt + F7  

[![](https://3.bp.blogspot.com/-a8xDiZUBz7E/WR6u2ELqhfI/AAAAAAAALyE/PWIYmQjGKwQbyAPo5y-57Ya3jkWbznZZgCK4B/s640/Show%2Busages.gif)](http://3.bp.blogspot.com/-a8xDiZUBz7E/WR6u2ELqhfI/AAAAAAAALyE/PWIYmQjGKwQbyAPo5y-57Ya3jkWbznZZgCK4B/s1600/Show%2Busages.gif)  

*   Go into class/method

F4

It's useful when you are tracing code...  

*   Go into implementations

CMD + Alt + B  

*   Extract...

I always use double shift to find this one... It's useful when you finish writing implementation and want to extract interface from it.  

*   Surround with

CMD + Alt + T

[![](https://3.bp.blogspot.com/-eeCAmhQgtSs/WR6vBz7nBwI/AAAAAAAALyM/jAXHnz45y4IBkUqBVqb0vajpMqhKypRAwCK4B/s640/Surround%2Bwith.gif)](http://3.bp.blogspot.com/-eeCAmhQgtSs/WR6vBz7nBwI/AAAAAAAALyM/jAXHnz45y4IBkUqBVqb0vajpMqhKypRAwCK4B/s1600/Surround%2Bwith.gif)  

*   Extend selection

Alt + ↑ and Alt + ↓

Before I know this one, I use mouse to select words or use Shift. Right now, I don't need it because this one is way more useful.  

[![](https://1.bp.blogspot.com/-8th1NQBygJA/WR6uhgQ2oAI/AAAAAAAALxo/glFqv0u9f5sG0GVmEYchDbus2faEaJOXwCK4B/s640/Extend%2Bselection.gif)](http://1.bp.blogspot.com/-8th1NQBygJA/WR6uhgQ2oAI/AAAAAAAALxo/glFqv0u9f5sG0GVmEYchDbus2faEaJOXwCK4B/s1600/Extend%2Bselection.gif)  

*   Move lines

CMD + Shift + ↑ or CMD + Shift + ↓  

[![](https://4.bp.blogspot.com/-jtdQPh_QBps/WR6vGVVO4FI/AAAAAAAALyU/HzTvc52yM6QGFNjDiO4i46X5Wp8OvR2bACK4B/s640/Move%2Blines.gif)](http://4.bp.blogspot.com/-jtdQPh_QBps/WR6vGVVO4FI/AAAAAAAALyU/HzTvc52yM6QGFNjDiO4i46X5Wp8OvR2bACK4B/s1600/Move%2Blines.gif)  

*   Multicursor

Alt + Shift + left mouse click  

[![](https://2.bp.blogspot.com/-k2CLuPqQAQU/WR6vIbi4OWI/AAAAAAAALyc/JNQANtxZo2otq-6DE8jGSLZPBnYvKbtlwCK4B/s640/Multicursor.gif)](http://2.bp.blogspot.com/-k2CLuPqQAQU/WR6vIbi4OWI/AAAAAAAALyc/JNQANtxZo2otq-6DE8jGSLZPBnYvKbtlwCK4B/s1600/Multicursor.gif)  

*   Reformat code

CMD + Alt + L  

*   Error Fix

Alt + Enter  

*   Upper/lower case

CMD + Shift + U  

[![](https://1.bp.blogspot.com/-hsMWSWIcayQ/WR6vNqJ69zI/AAAAAAAALyk/jZGgCJ9Zw2gIwI-sMnype6gbiQxoAaCegCK4B/s640/Upper%253Alower%2Bcase.gif)](http://1.bp.blogspot.com/-hsMWSWIcayQ/WR6vNqJ69zI/AAAAAAAALyk/jZGgCJ9Zw2gIwI-sMnype6gbiQxoAaCegCK4B/s1600/Upper%253Alower%2Bcase.gif)  

### Live Templates

*   Log Tag

type "logt", then press Tab  

[![](https://2.bp.blogspot.com/-Ov_wphLPPTQ/WR6vTEHl7cI/AAAAAAAALys/HW07ejPGB7AXySvT3OCUy2DCiBn7fbVDACK4B/s640/log%2Btag.gif)](http://2.bp.blogspot.com/-Ov_wphLPPTQ/WR6vTEHl7cI/AAAAAAAALys/HW07ejPGB7AXySvT3OCUy2DCiBn7fbVDACK4B/s1600/log%2Btag.gif)  

*   Logging

Type logd or logw and so on... then press Tab  

[![](https://4.bp.blogspot.com/-3UGYzUzNpeI/WR6vV83kxxI/AAAAAAAALy0/A-vaYoOGArAjaWCVuw-arJYeRwFRJb0ewCK4B/s640/Logging.gif)](http://4.bp.blogspot.com/-3UGYzUzNpeI/WR6vV83kxxI/AAAAAAAALy0/A-vaYoOGArAjaWCVuw-arJYeRwFRJb0ewCK4B/s1600/Logging.gif)  

*   Find View by Id

Type fbc, then press Tab  

[![](https://4.bp.blogspot.com/-ErZj-T8PZ6Y/WR6vZcgeMqI/AAAAAAAALy8/h4aSjP8fo2U9burozWy3ppyvqPwGlIFCQCK4B/s640/Find%2BView%2Bby%2BId.gif)](http://4.bp.blogspot.com/-ErZj-T8PZ6Y/WR6vZcgeMqI/AAAAAAAALy8/h4aSjP8fo2U9burozWy3ppyvqPwGlIFCQCK4B/s1600/Find%2BView%2Bby%2BId.gif)  

*   New Instance

Type newInstance, then press Tab  

[![](https://1.bp.blogspot.com/-ShxQz7qnjtw/WR6vefLXoDI/AAAAAAAALzE/5gDi1QbTBqUswtWLbWju3Kh1pyAWPIZUACK4B/s640/new%2Binstance.gif)](http://1.bp.blogspot.com/-ShxQz7qnjtw/WR6vefLXoDI/AAAAAAAALzE/5gDi1QbTBqUswtWLbWju3Kh1pyAWPIZUACK4B/s1600/new%2Binstance.gif)  

*   Visibility

Type visible, then press Tab  

[![](https://4.bp.blogspot.com/-PK2Uvf0RfAA/WR6vgw8IYNI/AAAAAAAALzM/GqIs141V00wzatBtMMhBp9loyyPT76tawCK4B/s640/visibility.gif)](http://4.bp.blogspot.com/-PK2Uvf0RfAA/WR6vgw8IYNI/AAAAAAAALzM/GqIs141V00wzatBtMMhBp9loyyPT76tawCK4B/s1600/visibility.gif)  

*   Toast

Type Toast, then press Tab  

[![](https://3.bp.blogspot.com/-tN-ykPUv8ZE/WR6vlNCSElI/AAAAAAAALzU/hKpgqHhEDmo3IR4g2C3Ms3SI3U0j7zgWQCK4B/s640/Toast.gif)](http://3.bp.blogspot.com/-tN-ykPUv8ZE/WR6vlNCSElI/AAAAAAAALzU/hKpgqHhEDmo3IR4g2C3Ms3SI3U0j7zgWQCK4B/s1600/Toast.gif)  

*   XML

For XML, Android Studio actually provide some templates for users to write code faster. For instance, "lwh" stands for " android:layout\_width="match\_parent" ". If you want to learn how to use them, please check out the preference of Android Studio, it has Live Templates section for you to set your templates.  

[![](https://3.bp.blogspot.com/-WiFm8S2gNCc/WR6vrJo-VDI/AAAAAAAALzc/rCur-jlpwswV0cCigWTUO0S3NrgPhbxlACK4B/s640/XML.gif)](http://3.bp.blogspot.com/-WiFm8S2gNCc/WR6vrJo-VDI/AAAAAAAALzc/rCur-jlpwswV0cCigWTUO0S3NrgPhbxlACK4B/s1600/XML.gif)  

### Postfix Completion

*   Cast

Cast your object faster  

[![](https://2.bp.blogspot.com/-EzEw87pgrh8/WR6v0N0F8rI/AAAAAAAALzk/LimjXdcTzOAHjLUp9-ScsGlJJxeOGWXYQCK4B/s640/cast.gif)](http://2.bp.blogspot.com/-EzEw87pgrh8/WR6v0N0F8rI/AAAAAAAALzk/LimjXdcTzOAHjLUp9-ScsGlJJxeOGWXYQCK4B/s1600/cast.gif)  

*   if & else

[![](https://3.bp.blogspot.com/--iCM9KofZTQ/WR6v3-13WyI/AAAAAAAALzs/LaDU58lsVj8oS9x_O-0Q-Yd59sOoMAXhwCK4B/s640/if%2B%2526%2Belse.gif)](http://3.bp.blogspot.com/--iCM9KofZTQ/WR6v3-13WyI/AAAAAAAALzs/LaDU58lsVj8oS9x_O-0Q-Yd59sOoMAXhwCK4B/s1600/if%2B%2526%2Belse.gif)  

*   Field

[![](https://2.bp.blogspot.com/-rkKoYKF7Ug4/WR6v7nQTCqI/AAAAAAAALz0/HC1DmQqq3J0U3SZ4GEN6HAywlFHN6QeVACK4B/s640/Field.gif)](http://2.bp.blogspot.com/-rkKoYKF7Ug4/WR6v7nQTCqI/AAAAAAAALz0/HC1DmQqq3J0U3SZ4GEN6HAywlFHN6QeVACK4B/s1600/Field.gif)  

*   For

[![](https://1.bp.blogspot.com/-tiucLnCndno/WR6v9ZkzDuI/AAAAAAAALz8/1DE82Axfa6Qj_In8wrGJcdwlHMohiuTeACK4B/s640/for.gif)](http://1.bp.blogspot.com/-tiucLnCndno/WR6v9ZkzDuI/AAAAAAAALz8/1DE82Axfa6Qj_In8wrGJcdwlHMohiuTeACK4B/s1600/for.gif)

  
[![](https://2.bp.blogspot.com/-m6pwTS7NZqI/WR6wDFllxGI/AAAAAAAAL0E/lnjpBp2Tm4wN39N9x-U_bbNd0XcPMBTJwCK4B/s640/fori.gif)](http://2.bp.blogspot.com/-m6pwTS7NZqI/WR6wDFllxGI/AAAAAAAAL0E/lnjpBp2Tm4wN39N9x-U_bbNd0XcPMBTJwCK4B/s1600/fori.gif)  

*   Null Check

[![](https://1.bp.blogspot.com/-n6Pg-whRc3o/WR6wLjYq37I/AAAAAAAAL0M/bCoOiRFwvNs9ipa-I3Y0s71jLBvnNJOYACK4B/s640/null%2Bcheck.gif)](http://1.bp.blogspot.com/-n6Pg-whRc3o/WR6wLjYq37I/AAAAAAAAL0M/bCoOiRFwvNs9ipa-I3Y0s71jLBvnNJOYACK4B/s1600/null%2Bcheck.gif)  

*   Try & Catch

[![](https://4.bp.blogspot.com/-RnqYDIAL03o/WR6xeXciF8I/AAAAAAAAL0Y/NVjSNOELrg4ZqWDCUcO-USr-wmQaz3UCgCK4B/s640/try%2B%2526%2Bcatch.gif)](http://4.bp.blogspot.com/-RnqYDIAL03o/WR6xeXciF8I/AAAAAAAAL0Y/NVjSNOELrg4ZqWDCUcO-USr-wmQaz3UCgCK4B/s1600/try%2B%2526%2Bcatch.gif)  

### Others

*   Key Prompter

This is a IntelliJ plugin to remind you keyboard shortcuts. Every time you use mouse instead of keyboard shortcuts, it immediately prompt a small window to let you know the shortcut. For some people this plugin might be annoying, but for me, I think this is a tool to help me memorize shortcuts, and my coding efficiency might be better. I firstly heard about this plugin from [Fragmented Android podcast](http://fragmentedpodcast.com/episodes/048/), and I immediately download this plugin to my Android Studio... By the way, the podcast is awesome... I think Android developers should listen. I'm a big fan of the podcast😂