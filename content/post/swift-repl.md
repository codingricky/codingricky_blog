+++
author = "Ricky Yim"
categories = ["swift", "repl", "apple"]
date = 2014-06-27T11:29:10Z
description = ""
draft = false
slug = "swift-repl"
tags = ["swift", "repl", "apple"]
title = "Swift REPL"
aliases = [
    "/swift-repl/"
]
+++

A Read-eval-print-loop (REPL) is a simple program that takes input and immediately gives user feedback. The new language from Apple, Swift comes complete with its own REPL shell. To use it follow these simple instructions in a terminal. This is assuming you already have Xcode6 installed.

1. $ sudo xcode-select -s /Applications/Xcode6-Beta2.app/Contents/Developer/
Change the path to point to the version of Xcode you have installed


2. $ xcrun swift 


3. Now should be in Swift's REPL and you can type commands that will be instantly executed.

```
$ var a = "hello"
$ println(a)
```

Happy REPL-ing!