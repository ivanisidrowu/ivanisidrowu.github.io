---
layout: post
title: MVVM Pattern and Android Data Binding
date: '2015-10-29T06:52:00.001-07:00'
author: Ivan
tags:
- Android
categories:
- Android
modified_time: '2015-11-20T09:38:32.154-08:00'
thumbnail: http://3.bp.blogspot.com/-KS7PwDKOLKg/VjH2ONgVzXI/AAAAAAAAASU/_KtnWFt1CUA/s72-c/device-2015-10-29-183346.png
blogger_id: tag:blogger.com,1999:blog-415098333644749081.post-2518705937891235446
blogger_orig_url: http://invictuscode.blogspot.com/2015/10/mvvm-pattern-and-android-data-binding.html
---

This year, google released [data binding library](http://developer.android.com/intl/zh-tw/tools/data-binding/guide.html). It's really interesting to use data binding on Android.  
  

### **Views and View Injection**

Normally, when we write codes in activity or fragment, we need to write a lot of boilerplate codes.  

 urlEditText = (EditText) rootView.findViewById(R.id.server\_domain);  
 portEditText = (EditText) rootView.findViewById(R.id.server\_port);  
 okButton = (Button) rootView.findViewById(R.id.server\_ok);  

Before data binding library comes out, I used [Butterknife](http://jakewharton.github.io/butterknife/) to make code concise and readable. Butterknife injects views into your activity or fragment, so you don't need to write "getViewById" or  bind some UI events like onClick by yourself.  

 @Bind(R.id.text\_stored) TextView mTextStored;  
 @Bind(R.id.btn\_refresh) Button mBtnRefresh;

 @OnClick(R.id.refresh)  
 public void onClick(View v) {  
    reloadData();  
 }  

Although Butterknife is good enough to avoid boilerplate codes, I personally think data binding is better. Data binding library allows us to bind views and data objects by simply modifying layout files and creating objects. What's more, you don't have to write many annotations or resource IDs. Does it sounds interesting?  
  

### MVVM

Before I explain how to implement data binding on Android, I wanna talk about MVVM. What is MVVM? It represents Model-View-ViewModel. Model is the object which represent data. View helps you show data, so it's basically UI component. ViewModel communicates with Model and View. It helps you to handle UI event and load data. For more information, you can check on the references below this article.  
  

#### Model

First step of implementation, you'll need to define Model, just like this one:  
  

public class Phone {  
    private String name;  
  
    public Phone(){  
  
    }  
  
    public Phone(String name){  
        this.name = name;  
    }  
  
    public String getName() {  
        return name;  
    }  
  
    public void setName(String name) {  
        this.name = name;  
    }  
}  

  
It's just a normal java bean object, or you can also write POJO.  
  

#### ViewModel

Define your ViewModel to show data on views.  
  

public class PhoneViewModel {  
  
    public final String name;  
  
    public PhoneViewModel(String name) {  
        this.name = name;  
    }  
  
    public String getName() {  
        return name;  
    }  
  
}  

  
This is a very basic view model, and if you want to handle some UI event, you can write in ViewModel as well. Then, put it into layout file. However, we haven't got there yet. So let's continue.  
  

#### View

Write layout file, and put your ViewModel into layout. The layout xml will be like this one:  
  
In <data>, <variable> describes which viewmodel you want to put into this layout, and define the package path in "type" attribute. For example, I put PhoneViewModel into this layout by naming them "viewModel". Also, I put @{viewModel.name} in the TextView to show the model name.  
  
After creating the layout file, we can use them in activity, fragment, or even adapter. In activity, you can get Binding object in this way.  

MainActivityBinding binding = MainActivityBinding.inflate(getLayoutInflater());  

  
Or you can get it in adapter...  

@Override  
    public MyAdapter.ViewHolder onCreateViewHolder(ViewGroup parent,  
                                                   int viewType) {  
        ItemLayoutBinding binding = DataBindingUtil.inflate(LayoutInflater.from(parent.getContext()), R.layout.item\_layout, parent, false);  
        return new ViewHolder(binding);  
    }  

  
Remember the name of binding object depends on the layout file. For instance, if your layout file, named "item\_layout.xml", the binding object will be "ItemLayoutBinding". You can reference it respectively.  
  
The code examples of this blog post are available on [my GitHub](https://github.com/ivanisidrowu/AndroidExamples/tree/master/AndroidDataBindingExample).  

[![](http://3.bp.blogspot.com/-KS7PwDKOLKg/VjH2ONgVzXI/AAAAAAAAASU/_KtnWFt1CUA/s400/device-2015-10-29-183346.png)](http://3.bp.blogspot.com/-KS7PwDKOLKg/VjH2ONgVzXI/AAAAAAAAASU/_KtnWFt1CUA/s1600/device-2015-10-29-183346.png)

  

### References:

[http://blog.stablekernel.com/mvvm-on-android-using-the-data-binding-library/](http://blog.stablekernel.com/mvvm-on-android-using-the-data-binding-library/)  
[http://tech.vg.no/2015/07/17/android-databinding-goodbye-presenter-hello-viewmodel/](http://tech.vg.no/2015/07/17/android-databinding-goodbye-presenter-hello-viewmodel/)  
[http://developer.android.com/intl/zh-tw/tools/data-binding/guide.html](http://developer.android.com/intl/zh-tw/tools/data-binding/guide.html)