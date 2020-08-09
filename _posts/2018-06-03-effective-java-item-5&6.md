---
layout: post
title:  "Effective Java Item 5&6"
date:   2018-06-03 10:00:00
author: Ivan
categories: 
- EffectiveJava
- Java
tags: Java
---

Avoid creating unnecessary objects

* Reuse immutable objects while you can.

```java
String s = new String("fasdfasdf")
```

Don't do this, it creates two strings each time it invokes. Instead, we should do...

```java
String s = "fasdfasdf"
```

* Prefer primitives to boxed primitives, and watch out for unintentional autoboxing.

```java
// Slow program
public static void main(String[] args) {
  Long sum = 0L;
  for (long i = 0; i <= Integer.MAX_VALUE; i ++) {
    sum += i;
  }
  System.out.println(sum);
}
```

This snippet creates 2^31 unnecessary Long instances. "sum" should be primitive to prevent creating unnecessary objects.


## Effective Java Item 6: Eliminate obsolete object references

I personally think this item is important to android development because many cases can lead to memory leak in android. For example, setting a OnClickListener can lead to memory leak and even more. Every thing that holds activity as reference should be checked really carefully.

* Nulling out object references should be the exception rather than the norm, so we should scope the reference.

```java
// Memoery leak example
public class LeakedListActivity extends ListActivity {
@Override
public void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);

    // Use an existing ListAdapter that will map an array
    // of strings to TextViews
    setListAdapter(new ArrayAdapter<String>(this,
            android.R.layout.simple_list_item_1, mStrings));
    getListView().setOnItemClickListener(new OnItemClickListener() {
        private final byte[] junk = new byte[10*1024*1024];
        @Override
        public void onItemClick(AdapterView<?> arg0, View arg1, int arg2,
                long arg3) {
        }
    });     
}
    private String[] mStrings = new String[] {"1", "2"};
}
```

This series of articles are notes when I am reading a book, called Effective Java (2nd version). If you are interested, you should definitely check it out.
