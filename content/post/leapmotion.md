+++
author = "Ricky Yim"
categories = ["Leap Motion", "Breakout", "Hack Day"]
date = 2014-01-25T11:23:40Z
description = ""
draft = false
slug = "leapmotion"
tags = ["Leap Motion", "Breakout", "Hack Day"]
title = "Leaping into Breakout!"
aliases = [
    "/leapmotion/"
]
+++

As part of my [company's](http://www.dius.com.au) inaugural Hack Day, I decided to finally do something with the [Leap Motion](http://leapmotion.com/) toy that had been sitting on my desk for quite some time. For the uninitiated, the Leap Motion is a motion sensor, tracking hand and finger movement. It has an SDK enabling one to use these movements as inputs in applications. It is actually quite a sophisticated gadget, as it can recognise both hands and fingers individually and as well as detect gestures like swiping and tapping. 

As I was limited to a day, I decided to choose an exercise that was small but would still yield a highly demonstrable outcome. I chose to add some gesture controls to a [Breakout](http://en.wikipedia.org/wiki/Breakout_(video_game)) clone. Basically the goal was to allow you to use your hand to control the paddle in the game. The code used in this post is Java, however there are many different language bindings for Leap Motion like Javascript and C++.

This post will show you the Leap Motion specific elements of the solution. 

## Setting it up

I started by forking an existing [Breakout clone](https://github.com/michaelskree/brick-attack) as I was not interested too much in writing the game as that was not the intent of the hack. I rearranged the project to be in a multi-module [maven](http://maven.apache.org/) project. This allowed me to seperate out the Leap Motion code from everything else.

It ended up looking like this:

```
\brick-attack-game
\brick-attack-leapmotion
\repository
```

Note that the source code to this post can be found [here](https://github.com/codingricky/brick-attack-leapmotion).

The **brick-attack-game** module contains the game which encompasses the graphics and game logic. The **brick-attack-leapmotion** module is an extension of the game but with Leap Motion controls attached. The **repository** module simply exposes the Leap Motion libraries as maven dependencies to be used by the *brick-attack-leapmotion* module. 

For a Java application to use Leap Motion, you need two files. The **LeapJava.jar** file which contains the Java API and the **libLeapJava.dylib** file which contains the native bindings for the library. This file name will vary depending on which operating system you are using. I use OSX, hence the **.dylib** extension. See the [Leap Motion developer site](https://developer.leapmotion.com/) for more information. These two files have been packaged up as part of the **repository** module, so when the **brick-attack-leapmotion** module depends on this module, these files are brought along as well.

## Using Leap Motion

The crux of everything that happens is in the **LeapMotionGame** class. This subclasses the original **Game** class and adds the Leap Motion initialisation logic and overrides the Game's **movePaddle** method to use Leap Motion for horizontal movement in lieu of the keyboard.

```Java
package com.github.codingricky.leapmotion;

import com.leapmotion.leap.Controller;
import org.skree.brickattack.Game;
import org.skree.brickattack.PaddleEntity;

public class LeapMotionGame extends Game {

    private static final int MOVEMENT_MULTIPLIER = 4;
    private final Controller controller;
    private final LeapMotionListener listener;

    public LeapMotionGame() {
        listener = new LeapMotionListener();
        controller = new Controller(listener);
    }

    @Override
    protected void movePaddle(PaddleEntity paddle) {
        paddle.setHorizontalSpeed(listener.getX() * MOVEMENT_MULTIPLIER);
    }

    public static void main(String[] args) {
        final LeapMotionGame leapMotionGame = new LeapMotionGame();
        leapMotionGame.gameLoop();
    }
}
```

The main thing to note with this class is you must keep references to the listener and controller objects and ensure they are not garabage collected. Failing to do so will cause your application to crash. 

Now let's take a look at the actual **LeapMotionListener** class.

```Java
package com.github.codingricky.leapmotion;

import com.leapmotion.leap.Controller;
import com.leapmotion.leap.Hand;
import com.leapmotion.leap.Listener;

public class LeapMotionListener extends Listener {
    private float x;

    @Override
    public void onFrame(Controller controller) {
        final Hand hand = controller.frame().hands().get(0);
        if (hand.palmPosition().isValid()) {
            x = hand.palmPosition().getX();
        } else {
            x = 0;
        }
    }

    public float getX() {
        return x;
    }
}
```

The listener simply gets the first hand in the frame and if that hand has a valid palm position, will store its X co-ordinate. Admittedly this is quite a simple use case, however the API does support more sophisticated operations like gesture detection and interrogating both hands.

## Running the Game

With this all in place, you should be able to run the game which is a simple class of launching the main class (LeapMotionGame). The only thing to note here is you need to set the **-Djava.library.path** to the path of the native library. I have included a [shell script](https://github.com/codingricky/brick-attack-leapmotion/blob/leap-motion/brick-attack-leapmotion/src/main/scripts/game.sh) that has an example of this. 


Here is a video of the game in action.

<iframe width="420" height="315" src="//www.youtube.com/embed/2eaM1zSOalQ" frameborder="0" allowfullscreen></iframe>

## Summary
Hopefully this post showed you how easy it is to play and experiment with the Leap Motion SDK. I hope you find an interesting use for it.

## Links
Here is the [source code](https://github.com/codingricky/brick-attack-leapmotion) to the application. The instructions to check it out and run it are in the [README](https://github.com/codingricky/brick-attack-leapmotion/blob/leap-motion/README.md).

Here are some related links on the topic.

* [https://developer.leapmotion.com/](https://developer.leapmotion.com/)
* [https://developer.leapmotion.com/documentation/Languages/Java/Guides/Sample_Java_Tutorial.html](https://developer.leapmotion.com/documentation/Languages/Java/Guides/Sample_Java_Tutorial.html)
* [http://dius.com.au/2013/11/11/dius-hack-day/](http://dius.com.au/2013/11/11/dius-hack-day/)
