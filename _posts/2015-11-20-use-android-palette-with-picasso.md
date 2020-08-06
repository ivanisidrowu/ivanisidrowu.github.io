---
layout: post
title: Use Android Palette with Picasso
date: '2015-11-20T17:59:00.002-08:00'
author: Ivan
tags:
- Java
- Android
categories:
- Android
modified_time: '2015-11-20T17:59:37.281-08:00'
thumbnail: http://4.bp.blogspot.com/-qUTxA_FqwSI/Vk_PzNllGUI/AAAAAAAAATk/QVkJHC7G-4k/s72-c/Screenshot_20151121-094418.png
blogger_id: tag:blogger.com,1999:blog-415098333644749081.post-4549403022714172816
blogger_orig_url: http://invictuscode.blogspot.com/2015/11/use-android-palette-with-picasso.html
---

Palette is an Android library that you can extract a small set of colors from a Bitmap. I first saw this feature, I thought this is really cool if we can use it dynamically on APP. So that we can build really good looking APP, and this is why I want to write this post.  
  
First step of using this lib, you need to write dependency on your gradle file...  
  

compile 'com.android.support:palette-v7:23.1.0'

  
In this example, we will use Picasso to help us loading images. If you haven't heard about it, you can check it out on the website. It's a really great library which can load images into view, and you can also write less code. Also, you don't need to worry about Bitmap or caching mechnism if you use Picasso. Picasso can take care of them!  
[http://square.github.io/picasso/](http://square.github.io/picasso/)  
  
The basic usage of Picasso could be very simple.  
  

Picasso.with(context).load("http://i.imgur.com/DvpvklR.png").into(imageView);  

  
The above code shows you how to load an image from an url, and it is sooooooo simple! But how to use it with Palette? Take a look at this piece of code...  

public void loadImage(final ImageView view, String imageUrl){  
        Picasso.with(view.getContext()).load(imageUrl).into(new Target() {  
            @Override  
            public void onBitmapLoaded(Bitmap bitmap, Picasso.LoadedFrom from) {  
                view.setImageBitmap(bitmap);  
                getPalette(bitmap);  
            }  
  
            @Override  
            public void onBitmapFailed(Drawable errorDrawable) {  
  
            }  
  
            @Override  
            public void onPrepareLoad(Drawable placeHolderDrawable) {  
  
            }  
        });  
    }  

  
It's also really simple, you can load the image into a Target to get the Bitmap. Once you got the Bitmap, you can use Bitmap to create Palette.  

public void getPalette(Bitmap bitmap){  
        Palette.from(bitmap).generate(new Palette.PaletteAsyncListener() {  
            @Override  
            public void onGenerated(Palette palette) {  
                // here's the palette  
            }  
        });  
}  

The way we get the palette is an asynchronize method, but if you want to obtain palette synchronically, you can get it in this way...  

Palette.from(bitmap).generate();  

Remember, generating palette might take a while, so it's better to put it in background thread to execute it, and get it in listener. After getting the palette, we can get different colors from the palette and use them in UI. Palette default has 6 color profiles.  

int vibrantColor = palette.getVibrantSwatch().getRgb();  
int vibrantDarkColor = palette.getDarkVibrantSwatch().getRgb();  
int vibrantLightColor = palette.getLightVibrantSwatch().getRgb();  
int mutedColor = palette.getMutedSwatch().getRgb();  
int mutedDarkColor = palette.getDarkMutedSwatch().getRgb();  
int mutedLightColor = palette.getLightMutedSwatch().getRgb();  

However, this is not perfect, sometimes the methods to get Swatch objects could return null. To avoid NullPointerException, we can check null before get RGB colors.  

if(darkVibrantSwatch != null){  
   tabs.setBackgroundColor(darkVibrantSwatch.getRgb());  
}

  
I wrote an example APP to demonstrate how to use Palette, you can check it out in my github repository.  
[https://github.com/ivanisidrowu/AndroidExamples/tree/master/Palette](https://github.com/ivanisidrowu/AndroidExamples/tree/master/Palette)  
  

[![](http://4.bp.blogspot.com/-qUTxA_FqwSI/Vk_PzNllGUI/AAAAAAAAATk/QVkJHC7G-4k/s640/Screenshot_20151121-094418.png)](http://4.bp.blogspot.com/-qUTxA_FqwSI/Vk_PzNllGUI/AAAAAAAAATk/QVkJHC7G-4k/s1600/Screenshot_20151121-094418.png)

  
  
Finally, we can use them on our APPs, and TA-DA~ The UI is looking good, doesn't it?  
  

[![](http://2.bp.blogspot.com/-7KwEh6SEpg8/Vk_Bkm27cnI/AAAAAAAAATQ/0T9Di1abBfw/s640/Screenshot_20151121-014152.png)](http://2.bp.blogspot.com/-7KwEh6SEpg8/Vk_Bkm27cnI/AAAAAAAAATQ/0T9Di1abBfw/s1600/Screenshot_20151121-014152.png)