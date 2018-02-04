---
layout: post
title : "Referencing Attributes vs Resources"
subtitle : "The difference between ? and @ referencing in XML. And a few other tales from XML-land"
date: 2018-02-04 12:00:00
author: "Shikher Verma"
header-img: "img/posts/android-bg.jpg"
comments: true
tags: [CodeMonkey]
---

As most of you already know android uses XML files to define UI. Android's XML usage is quite straightforward (although Data Binding library makes it more interesting). Most developers learn it when they get started with android and never dig deeper into it. There are some fine details in the android XML beyond the basic. Which can and are mostly ignored by developers because you can live without knowing them. But knowing them is immensely useful because it shows you what all is possible. Like the app architecture has matured over the last few years, soon other areas like android styling will also mature, with more and more discussion on what the standard practise for styling should be. And to question what the standard practise should be, first we have to know the possibilities available to us.

### ? and @ referencing in XML
There are multiple ways to reference in android XML. Most probably you would have come across these two variants:
```
android:textColor="@color/colorPrimary"
android:textColor="?android:attr/colorPrimary"
```
This difference in referencing is one of the crucial details, prerequisite to styling a great app.

`@`  is used to reference an actual resource value. Think of it being `call by value`. `@color/colorPrimary` points to the line `<color name="colorPrimary">#333333</color>` in `res/values/color.xml`. Most references in XML are of this type.

`?` on the other hand is reference to a style attribute. The actual resource value pointed at, by this style attribute, depends on the current theme. So it's similar to Java's `call by reference`. The advantage is that you can dynamically change them without changing your XML. `?attr/colorPrimary` points to the current theme's line `<item name="colorPrimary">@color/colorPrimary</item>` in `res/values/style.xml`.

`?` referencing is less common. And most of the time when we use it, we reference something that is defined in android's inbuilt themes, not the ones defined by us. You must remember using it this way to set `textAppearance` in `TextView`
```
android:textAppearance="?android:attr/textAppearanceSmall"
android:textAppearance="?android:attr/textAppearanceMedium"
android:textAppearance="?android:attr/textAppearanceLarge"
```
Or using it to `style` a `ProgressBar` with `style="?android:attr/progressBarStyleSmall"`

**The syntax for referencing**

`@[package:]type/name`
* `package` (optional) is name of the package this resource is in. Default is the app package this resource file is in.
* `type` is one of `color`, `string`, `dimen`, `layout`, `anim` or some other resource type.
* `name` is the name given to the actual resource.

`?[package:]type/name`
* `package` (optional) is name of the package this resource is in. Default is the app package this resource file is in.
* `type` (optional) is always `attr` when referencing attributes. That is why it's optional.
* `name` is the name given to the actual resource.

### Other tales from XML-land
**Attribute Namespaces**

What's the difference between `android:attribute`, `app:attribute` and `tools:attribute`?

When I started building android stuff, I accepted `android:attribute` as a fixed syntax for declaring attributes. Never questioned what the suffix `android:` meant. Until I started using `AppCompat`. After coming across `app:attribute` I had to find out what exactly was going on. These are namespaces of attributes and depending on which namespaces you want to use, you will have to declare the corresponding XML namespace (in short `xmlns`) in the root view.
```
xmlns:android="http://schemas.android.com/apk/res/android"
xmlns:app="http://schemas.android.com/apk/res-auto"
xmlns:tools="http://schemas.android.com/tools"
```
This is what each namespace does:
* `android:` namespace includes all the attributes provided by the android system itself.
* `app:` namespace includes the attributes provided by your app and the libraries used in your app. That is why `AppCompat` attributes end up being `app:`. You can easily make custom attributes for yourself using the `DataBinding` library or by making custom views.
* `tools:` namespace provides design time features. Since most views are filled with data dynamically while running, the android UI editor is unable to show what a layout would look like when the app is running. `tools:` provides attributes which can define layout characteristics that are visible only in the Android Studio layout preview! You will be amazed by how much difference it makes.

**Include & Merge tags**

One of the underappreciated features of android XML is ability to reuse layout components. Common components can be separated into their own XML file and can be included with `<include layout="@layout/component"/>` Titlebar is the most common component which is separated into its own layout. Other components such as floating action button or bottom navigation bar can be separated too. The include tags makes this modularity possible.

But a problem with include tag is that if you want to separate more than one view into a single XML file, you will have to wrap them with a root view. This root view serves no real purpose other than to slow down your UI performance. `<merge></merge>` can be used as a root view in such cases as the compiler automatically removes it.

I hope you found the blog useful. If you think I missed something important or I was wrong somewhere, please let me know through the comments :)
