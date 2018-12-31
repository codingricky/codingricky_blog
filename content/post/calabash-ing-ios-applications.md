+++
author = "Ricky Yim"
categories = ["Cucumber", "testing", "ios", "calabash"]
date = 2014-10-30T00:20:14Z
description = ""
draft = false
slug = "calabash-ing-ios-applications"
tags = ["Cucumber", "testing", "ios", "calabash"]
title = "Calabash-ing iOS applications"
aliases = [
    "/calabash-ing-ios-applications/"
]
+++

Automated acceptance testing is a standard and common practice when developing web applications. One of the most popular frameworks is [Cucumber](http://cukes.info/) and it is easy to integrate this into Ruby on Rails and Java applications. However, nowadays it is just as easy to use these frameworks with native mobile applications. This article will take a trivial application written in Swift and show how it is possible to use a framework like [Calabash](http://http://calaba.sh/) and begin writing automated acceptance tests.

Before proceeding, these are the pre-requistes required to run the software described in this article:

* Xcode 6.1
* Git
* Ruby
* Rubygems

#### To Do Lists
The application that we will add acceptance tests to is a simple To Do List. The source code can be found [here](https://github.com/codingricky/todoSwift). I forked an existing [application](https://github.com/IcaliaLabs/todoSwift) and made a few minor modifications.

1 . Checkout the repository, you should be by default on the **initial** branch. 

```
$ git clone https://github.com/codingricky/todoSwift
```

2 . Open the file **SingleTodo.xcodeproj** in Xcode. Compile and run this.

You should be presented with an application that looks like the following:

![](/content/images/2014/10/iOS-Simulator-Screen-Shot-29-Oct-2014-9-45-29-pm.png)

Clicking **Add** will add a task. Clicking **Done** will remove any checked tasks.

Now that we have a functioning application, let's add some tests.

### Calabash

Calabash is one of many acceptance testing frameworks for iOS. It is the one that I have found most easy to use. Given my background with Rails and Cucumber, the transition was seamless as Cucumber is supported in the Calabash framework. One of the negatives of Calabash is that it requires a separate version of the application to be built, as it embeds an HTTP Server in the application. This enables the automation magic to happen.

3 . Calabash is distributed as a gem. Issue the following command in a terminal.

```
$ gem install calabash-cucumber --no-ri --no-rdoc
```

4 . Install the Calabash framework into the Xcode project by executing the following command in the directory that you cloned the git repository.

```
$ calabash-ios setup
```

This will add another scheme called **SingleTodo-cal** to the Xcode project. It will add in the calabash.framework directory too.

```
$ ls -al
total 32
drwxr-xr-x  10 ricky  staff   340 30 Oct 11:31 .
drwxr-xr-x   4 ricky  staff   136 30 Oct 11:25 ..
drwxr-xr-x  13 ricky  staff   442 30 Oct 11:32 .git
-rw-r--r--   1 ricky  staff   289 30 Oct 11:25 .gitignore
-rw-r--r--   1 ricky  staff    57 30 Oct 11:25 .travis.yml
-rw-r--r--   1 ricky  staff  1478 30 Oct 11:25 README.md
drwxr-xr-x  12 ricky  staff   408 30 Oct 11:25 SingleTodo
drwxr-xr-x   5 ricky  staff   170 30 Oct 11:25 SingleTodo.xcodeproj
-rwxr-xr-x   1 ricky  staff    92 30 Oct 11:25 build.sh
drwxr-xr-x   6 ricky  staff   204 24 Oct 13:10 calabash.framework
```


5 . Generate a sample feature file. Run the following command:

```
$ calabash-ios gen
```

This will have generated the following files.

```
$ ls features/
my_first.feature	step_definitions	support
```

6 . Build the **SingleTodo-cal** scheme in Xcode. Run the sample test.

```
$ cucumber
```

This will execute the feature against the simulator. An actual simulator will pop up. The output in the terminal should resemble the following:

```
Feature: Running a test
  As an iOS developer
  I want to have a sample feature file
  So I can begin testing quickly

  Scenario: Example steps                            # features/my_first.feature:6
    Given I am on the Welcome Screen                 # features/step_definitions/my_first_steps.rb:1
    Then I swipe left                                # calabash-cucumber-0.11.3/features/step_definitions/calabash_steps.rb:234
    And I wait until I don't see "Please swipe left" # calabash-cucumber-0.11.3/features/step_definitions/calabash_steps.rb:165
    And take picture                                 # calabash-cucumber-0.11.3/features/step_definitions/calabash_steps.rb:229

1 scenario (1 passed)
4 steps (4 passed)
0m31.562s
```

### Actual Test

It is fine having a sample test however let's write a test that is a bit more relevant to the application.

To keep things simple, we will use the [predefined steps](https://github.com/calabash/calabash-ios/wiki/02-Predefined-step) in Cucumber/Calabash. The test will add a task and assert that it has been added.

7 . Create a new feature file and delete the sample one.

```
$ touch features/todo.feature
$ rm features/my_first.feature
```

8 . Edit your new file (**features/todo.feature**) in your preferred editor. Paste the following into the file.

```
Feature: Todo list application can add tasks
  As a user
  I want to add tasks
  So I can remember to do things

Scenario: Add Task
  Given I am on the Welcome Screen
  When I touch "Add"
  And I fill in "TaskDescription" with "listening to Taylor Swift"
  And I touch "Add task"
  Then I wait for "listening to Taylor Swift" to appear
```

The test is pretty self explanatory. There are a couple of things to note. Firstly, the **"When I touch"** steps use the label of the button. The **"fill in"** step uses an Accessibility Label. Since we're learning Swift we might as well be listening to some Swift. 

9 . Run cucumber again, the scenario should execute.

```
$ cucumber
```

Output should resemble the following:

```
Feature: Todo list application can add tasks
  As a user
  I want to add tasks
  So I can remember to do things

  Scenario: Add Task                                                 # features/todo.feature:6
    Given I am on the Welcome Screen                                 # features/step_definitions/my_first_steps.rb:1
    When I touch "Add"                                               # calabash-cucumber-0.11.3/features/step_definitions/calabash_steps.rb:15
    And I fill in "TaskDescription" with "listening to Taylor Swift" # calabash-cucumber-0.11.3/features/step_definitions/calabash_steps.rb:114
    And I touch "Add task"                                           # calabash-cucumber-0.11.3/features/step_definitions/calabash_steps.rb:15
    Then I wait for "listening to Taylor Swift" to appear            # calabash-cucumber-0.11.3/features/step_definitions/calabash_steps.rb:174

1 scenario (1 passed)
5 steps (5 passed)
0m34.296s
```

### Summary

So now you've seen how easy it is to get up and running with Calabash and iOS applications. You can easily build up a suite of tests with this approach and ensure the quality of your application remains high. 

In future articles, I will build upon this example, so keep an eye out!

### References

* https://github.com/calabash/calabash-ios
* http://calaba.sh/
* https://github.com/calabash/calabash-ios/wiki/02-Predefined-step

