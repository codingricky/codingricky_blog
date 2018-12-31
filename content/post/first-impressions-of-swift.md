+++
author = "Ricky Yim"
date = 2014-06-16T12:09:35Z
description = ""
draft = false
slug = "first-impressions-of-swift"
title = "First impressions of Swift"
aliases = [
    "/first-impressions-of-swift/"
]
+++

**First impressions of Swift**

I’ve spent most of my idle cycles over the last week or so having a closer look at Swift, the new Apple programming language that was announced recently at WWDC. Here is a quick summary of what I like and dislike so far. Mind you, this is only after having a fairly brief look into Swift, so my opinions may change…

**Likes**

* Optional/Inferred Typing - I really like this feature. I wish Java would adopt something similar. From my point of view, less typing means less code means less bugs
* No header files - I always felt that I was stuck in the 80s when I was programming Objective-C with header files. Now they’re not necessary in Swift. Function declaration and implementation in the one spot
* let vs var - Allows the developer to enforce some degree of immutability in programs
* Named arguments - Particularly in functions with long argument lists, this comes in very handy
* String interpolation - A neat little feature for replacing arguments in Strings
* String construction - I hated the whole @"String" business in Objective-C, this has been done with in Swift
* Interoperability with Objective-C - A few times I’ve had to include Objective-C files into my Swift project. I must say the ease in which this was done was impressive. All I needed was to define an Objective-C Bridging/Header file and import the header files that I required
* Readable - As Swift has a much closer relationship with modern languages like Scala and Ruby, I just find it much more readable than Objective-C

**Dislikes**

* ARC vs GC - Swift has support for Automatic Reference Counting (ARC) as opposed to Garbage Collection (GC). Granted ARC is a step up from leaving memory management entirely in the developer’s hands, however, I am concerned that there will be scenarios like (cyclic references) where developer’s will tie themselves in knots. I guess it is no different from Objective-C
* Lack of scope - In most modern programming languages, there’s an ability to define scopes for your functions and variables. This is not possible with Swift and I’m sure this feature will be missed once projects become larger and you want to tightly control how objects interact with each other
* Lack of data structures - Out of the box, Swift has support for 2 data structures, Array and Dictionary. I would have preferred several more like Stacks, Queues, Linked Lists, etc
* Error handling - Typically there is some try/catch, begin/rescue syntax built into the language to handle exception/error handling. There’s none of this in the language. See here for a solution to this problem (http://swiftlytyping.tumblr.com/post/88210131086/error-handling). Still I think it's something that should really be in the language