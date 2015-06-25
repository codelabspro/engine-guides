---
layout: default
title: Easing curves
---

<span class="intro-graf"> By default, [Transitionables](./transitionables.html) create constant or _linear_ transitions between start and end values. However, we can create more lively transitions by changing the Transitionable's _easing curve_.</span>

##Default curve

When no curve is specified, Transitionables use a linear _easing curve_. This means values transition evenly over the course of the specified duration.

    var child = context.addChild();

    var position = new Position(child);
    position.set(0,-300,0, { duration: 3000 })

 _Refer to the the previous [Transitioning Components](./transitionables.html) for an explanation of the code above_

Without setting the curve, halfway through this animation (1.5 seconds) the Y position value will be -150.

But, what if we wanted a more satisfying transition- maybe we want it to move faster at first and then slow down just at the end? This is where we use curves.

## Adding easing curves

Specify the easing curve in the options hash passed to the Transitionable's `.set()` method.

    position.set(0, 0, -300 { duration: 3000, curve: 'outQuad' });

Though completing in the same amount of time, the animation above now looks very different from the previous one.

For the specific case of quaternion interpolation, `method: 'slerp'` may be specified in the options hash to give a more natural rotational transition.

    var myValue = new Transitionable([1,0,0,0]);
    myValue.set([Math.cos(Math.PI/4),Math.sin(Math.PI/4),0,0], {duration: 1000, curve: 'outQuad', method: 'slerp'});


You may be wondering what other curves are at your disposal? Reference the table below or check out the [Curves.js](https://github.com/Famous/mixed-mode/blob/master/transitions/Curves.js) file for a full list of registered curves.


## Custom easing curves

Of course we can write and input our own curves. In order to avoid jitter, or a teleporting Transitionable, it is important that our custom functions are 1. continuous, and 2. begin at 0 and end at 1.


    function myCustomCurve(progress) {
        return Math.sin(progress * Math.PI/2);
    }

    position.set(0, 0, -300 { duration: 300, curve: myCustomCurve });

Note how the curve must accept a `progress` parameter, which varies linearly from 0 to 1 and describes the current stage of the transition.


## Easing curves table

Below is a full list, graph and demos for all 30 of the easing curves included by default with Famous.


<iframe src="https://s3-us-west-2.amazonaws.com/learn-staging.famo.us/container-demos/easing/index.html" style="width:100%;height:3000px"></iframe>


<div class="next-prev-container"><a rel="prev" class="previous" href="../pages/transitionables.html">Transitionables </a><a rel="next" class="next" href="../pages/program-events.html">Program Events </a></div>