---
layout: post
title : "Practical Data Structures Guide for Android developers"
subtitle : "Let’s explore some of the most used data structures in Android Development!"
date: 2017-10-25 20:13:00
author: "Shikher Verma"
header-img: "img/posts/android-bg.jpg"
comments: true
tags: [CodeMonkey]
---

Until a few months ago while developing android application I never thought much about data structures. This is partly because there are many more problems to solve while developing for android as compared to competitive programming where only running time matters. And partly because I started android development before coming to college, learning data structures and algorithms. Many of the habits and thought processes I developed during the initial days have stuck. I would dismiss data structures thinking that it would be trading code clarity for a little bit of efficiency and since we generally don't have to handle a lot of data in apps, it would make sense to stick to the very basic data structures and keep code maintainable. But this opinion was wrong.

The reasons why now I think choosing the right data structure is important:
1. Every data structure puts constrains on how you can access your data.
2. Choosing a more constrained data structure makes it more obvious how you are going to access the data. Reduces the [chances of astonishing](https://en.wikipedia.org/wiki/Principle_of_least_astonishment) your fellow programmers. Resulting in cleaner, more maintainable code.
3. Programming abstractions (interfaces, abstract classes, inheritance) put constrains on what you can and cannot do. All programming constrains minimize the chances of you shooting your own foot.
4. Choosing the right data structure leads you to the right algorithm so that also improves efficiency as a side effect.
5. And lastly it’s scary how many resort to `ArrayList` no matter the task. I was guilty of this too.

#### Arrays
Use an `Array` rather than `ArrayList` when the number of elements is fixed. The most important reason to do this is that this changes your fixed length constrain from words to code. The `[]` syntax clearly speaks out that this is a normal array with a fixed length to your fellow developers. Another reason which is a bit subjective is that `[]` syntax is more compact than `get()`, `add()` functions.

#### Lists
`ArrayList` itself uses `Array` to actually store the data. Then how can it magically have changing lengths? The [documentation](https://docs.oracle.com/javase/7/docs/api/java/util/ArrayList.html) says  
> As elements are added to an ArrayList, its capacity grows automatically. The details of the growth policy are not specified beyond the fact that adding an element has constant amortized time cost.

Constant amortized time is just a fancy way of saying that it is constant in most of the cases. If you look at the source code, it turns out `ArrayList` [starts with default length 10 ](https://github.com/openjdk-mirror/jdk7u-jdk/blob/master/src/share/classes/java/util/ArrayList.java#L139) and [increases by 50%](https://github.com/openjdk-mirror/jdk7u-jdk/blob/master/src/share/classes/java/util/ArrayList.java#L204) every time it reaches full capacity. So if you declare a new `ArrayList` and proceed to fill 100 elements. It would actually create arrays of length 10, 15, 22, 33, 50, 75, 113 !! And every time the capacity is exceeded it would copy over all the elements in the last array to the new one.
Inserting and removing an element are also costly operations. Say you remove element 10 from an `ArrayList` of length 100. All the elements from position 11 onward would have to be shifted left! Similarly inserting would shift all elements to the right.

Enter `LinkedList`, it does not need to move all the elements of the list to insert or remove an element in between. Adding an element is always constant time as it actually has dynamic capacity. Other operations such as `indexOf` have same cost in both list implementations. And `sort` costs the same for all `Collections` because the sort method first [dumps everything into an array](https://github.com/openjdk-mirror/jdk7u-jdk/blob/master/src/share/classes/java/util/Collections.java#L154). But `LinkedList` comes with an added constrain of not being able to access elements randomly. You can do `get(index)` but it would be a costly operation. This is the only place where `ArrayList` is better than a `LinkedList`. And I believe `ArrayList` should only be preferred over a `LinkedList` if random access is a necessity. More often than not I find that my code is using this feature due to my own laziness. Not because it was the best approach. To counter this `ArrayList` addiction start with initializing all your lists as `LinkedList` and only switch to an `ArrayList` if you can't live without random access. If you are going to fetch a list of posts from a server and then iterate over them, using a `LinkedList` would make your code more elegant. Since instead of `list.get(i)` you would write `list.next()` `list.previous()` which is much more easier to understand that seeing a `list.get(i)` and then trying to figure out how `i` is changing. Using an `ArrayList` is justified for a music app where the user might hit the random button and so `i` has to be randomly changed. 

#### Maps
The second most used data structure in android dev is `HashMap`. Like the `Array` vs `ArrayList` argument. Use a `EnumMap` over a `HashMap` if the keys are known before hand. They provide the similar advantages in code maintainability by being more constraint. And as a side effect, your app would be a little faster.  
Iterating over a `HashMap` provides no guaranty of the order in which elements will be traversed. Say you are building a shopping cart and store the orders in a map of product -> quantity. The user now wants to see all the items in his cart. Since a `HashMap` is being used, the card would show all the items but the order would be random. Instead using a `LinkedHashMap` is much better here, since you can iterate over it like a normal `LinkedList`. Using a `LinkedHashMap` here to show the products in the order they were added would be nice.  
Other useful `HashMap` variants are `WeakHashMap` and `TreeMap`. `WeakHashMap` stores only a [weak reference](https://web.archive.org/web/20061130103858/http://weblogs.java.net/blog/enicholas/archive/2006/05/understanding_w.html) to its keys which can help prevent memory leak. For example, you are storing `View` references in a `HashMap`, which is being shared between `Activity` and background `Service`. You would want the `View` references to be garbage collected when the `Activity` finishes. If you use a normal `HashMap` in this case it would hold a strong reference causing memory leak. `TreeMap` is useful when you want entries to be sorted by key.

#### Infrequently used Data Structures
*Sets*  
If you know that a list is only going to have unique elements use a `HashSet` instead of a `ArrayList`.
As in the case of `Array` vs `ArrayList`, `EnumMap` vs `HashMap` we also have `EnumSet` vs `HashSet`. For example if you have a list of configuration keys you can represent the "selected" keys use an `EnumSet`. If order is important, similar to `LinkedHashMap`, here we have `LinkedHashSet` which preserves insertion order and `TreeSet` which keeps the elements sorted.

*Trees*  
Your data would hardly ever be of the form of a `Tree` but one very frequent use case is that of nested comments. I naively implemented nested comments using an `ArrayList` once. As a result all operations on the data (adding, removing, editing a comment) because incredibly complex. For example, removing a comment would be simply removing the comment node from the tree data structure, but since it was stored in a `ArrayList` I had to iterate forward from the index of the comment to remove all its children too. The work that should have been abstracted away by the data structure was now being handled when the data structure was used. Unfortunately java does not have a general tree implementation.

*Stacks and Queues*  
To be honest I have never used a `Stack` during android development. However they are used in Android itself (for example the activity back stack). I did `Queue` use, once in a background `IntentService`, to buffer work requests. `Stack` and `Queue` have very rare use during android development. But there are still important to know just in case the use ever comes up. Have you every used these during development ?

#### Concurrency
Concurrency is an important issue too while choosing the data structure which is very much ignored. When the same data structure is modified and used in two different threads they should be synchronized. Concurrency issues rarely happen but when they do its very difficult to point it out since they are hard to reproduce. I have mostly encountered multi-threading using `AsyncTasks`, `Runnables`, `Executor`,  `Handlers`, `Service` or using `rxjava`. As a thumb rule, when using these classes simply check if you are modifying a data structure from any thread aside from the `UI thread`. You need to use a concurrency compatible version of your data structure, even if you are not modifying the data structure but modifying the elements in it.

#### Inheriting from these data structures
Finally if you have some special constrains on the data, its better to have a separate class to encapsulate it by extending from the in built data structures. Say for example you want to have an `ArrayList` of `File` objects but you constrain it to only store `File`s that actually exist. You could check every time before adding a file to the list. Or you could have a new class `ExistingFileList` which extends from `ArrayList` and wraps the add function.

For more in depth analysis of data structures and algorithms I recommend [Prof. Clifford A. Shaffer's Algorithm book](http://courses.cs.vt.edu/cs3114/Spring09/book.pdf). I like this book because it does not ignore other aspects of software development like most resources of the data structures and algorithm resources on the internet.

If you think I missed something important or I was wrong somewhere, please let me know through the comments :)
