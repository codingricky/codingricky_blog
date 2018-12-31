+++
author = "Ricky Yim"
categories = ["apple", "swift", "ios", "access control"]
date = 2014-07-29T02:28:47Z
description = ""
draft = false
slug = "access-control-in-swift"
tags = ["apple", "swift", "ios", "access control"]
title = "Access Control in Swift"
aliases = [
    "/access-control-in-swift/"
]
+++

Since Swift was released at WWDC this year, Apple have clearly stated the language is still in beta. This was proven by the fact that the last release, beta 4, introduced a new feature, *"Access Control."* Initially when Swift was released, this did seem like a glaring omission as almost every modern programming language today has some notion of Access Control.

### 3 levels

Swift has 3 levels of Access Control.

1. **private** - Access from only the same file
2. **internal** - Access from the same module
3. **public** - Across modules

Let's start with **private** the most restrictive one. This will stop anybody accessing anything that is not in the same file.

For example, if I had a **Person** class with a **name** variable that were **private**, nobody else outside the file would be able to access it.

So in **Person.swift**
```swift
class Person {
    private let name: String
    
    init(name: String) {
        self.name = name
    }
}
```

And in **main.swift**

```swift
let person = Person(name: "John")
println(person.name) // compile error 
```

Removing the **private** keyword from the variable, removes the compile error. 

So again in **Person.swift**
```swift
class Person {
    let name: String
    
    init(name: String) {
        self.name = name
    }
}
```

In **main.swift**
```swift
let person = Person(name: "John")
println(person.name) // prints out John
```

##Internal 

So when we removed the **private** keyword from our **Person.swift** class it had **internal** scope by default. This is the same as adding the **internal** keyword to the attribute. 

In **Person.swift**
```swift
class Person {
    internal let name: String
    
    init(name: String) {
        self.name = name
    }
}
```

##Public 
Lastly, **public** should be used with modules and when exposing APIs. So if you want your functions/attributes to be accessible across modules, then use the **public**. 

### Summary
I hope you've learnt something about Swift's access controls in this post. Here are some references for further reading.

* https://developer.apple.com/swift/blog/?id=5
* https://developer.apple.com/swift/

The source code for this post can be found here https://github.com/codingricky/Swift-Access-Control-Demo.