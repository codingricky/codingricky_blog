+++
author = "Ricky Yim"
categories = ["swift", "initializers", "delegation", "convenience", "designated", "ios"]
date = 2014-07-07T12:27:18Z
description = ""
draft = false
slug = "construtor-chaining-in-swift"
tags = ["swift", "initializers", "delegation", "convenience", "designated", "ios"]
title = "You say Constructor Chaining, Swift says Initializer Delegation"

+++

One of the things that one must get used to with a new language is dealing with new conventions. Now coming from mostly a Java and Ruby background, there is a notion of constructors and constructor chaining, basically when one constructor calls another one. So when it comes to Swift, there are a few rules around the methods used to create objects.

Firstly, the [language guide](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/TheBasics.html#//apple_ref/doc/uid/TP40014097-CH5-XID_399) refers to constructors as initializers. So basically anytime you want to use the term constructor, use the term initializer when you are in Swift land. More specifically, constructor chaining is known as initializer delegation. 

Initializers fall into two categories, *Designated* and *Convenience*.  *Designated* initializers are the primary initializers and are responsible for initializing all properties of a class. So in this case, the only initializer here is known as the *Designated* one. 

```swift
class Person {
    let name: String
    let age: Int
    
    init(name: String, age: Int) {
        self.name = name
        self.age = age
    }
}
```

Now if you want to add other initializers to delegate to the *Designated* one, they will be known as *Convenience* initializers. So let's add one that defaults the age. *Convenience* initializers have the keyword *convenience* before them.

```swift
class Person {
    let name: String
    let age: Int
    
    init(name: String, age: Int) {
        self.name = name
        self.age = age
    }
    
    convenience init(name: String) {
        self.init(name: name, age: 100)
    }
}
```

*Convenience* initializers need to delegate to another *Convenience* initializer or a *Designated* initializer. 

Here's an example of a *Convenience* initializer calling another *Convenience* one.

```swift
class Person {
    let name: String
    let age: Int
    
    init(name: String, age: Int) {
        self.name = name
        self.age = age
    }
    
    convenience init(name: String) {
        self.init(name: name, age: 30)
    }

    convenience init() {
        self.init(name: "Homer")
    }
}
```
If you wanted to, you could create another *Designated* initializer. Now we have two *Designated* initializers and two *Convenience* initializers.

```swift
class Person {
    let name: String
    let age: Int
    
    init(name: String, age: Int) {
        self.name = name
        self.age = age
    }
    
    init(age: Int, name: String) {
        self.age = age
        self.name = name
    }
    
    convenience init(name: String) {
        self.init(name: name, age: 30)
    }
    
    convenience init() {
        self.init(name: "Homer")
    }
}
```

###What about subclasses?

Once again, with initializers, there are certain rules that need to be adhered to when subclassing. So *Designated* initializers must call other *Designated* initializers in their immediate parent class.

So let's take a look at an example. We are adding a Student class as a subclass of the Person class. It will add a property student number.

```swift
class Student : Person {
    
    let number: String
   
    init(name: String, age: Int, number: String) {
        self.number = number
        super.init(name: name, age: age)
    }
}
```

Here we are calling the *Designated* initializer in the Person class. If we tried to call one of the *Convenience* initializers in the Person class, a compile error would occur. Also note, we need to assign the number property before calling the initializer in the parent class as it is a requirement to ensure all properties are initialized in child classes before their respective parent initializer is called.

###Summary

Basically in summary, as explained in the language guide, *Designated* initializers delegate up and *Convenience* initializers delegate across. I hope this article helped in some way to improve your understanding of initializers in Swift.

###References
* [Swift Language Guide](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/TheBasics.html#//apple_ref/doc/uid/TP40014097-CH5-XID_399)


