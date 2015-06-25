---
layout: default
title: Transitionables
---



<!-- TODO: pause/resume, callbacks -->

<span class="intro-graf"> Generating the intermediate or _in between_ frames of an animation is often referred to as _tweening_. In Famous, we can create tween animations using Transitionables.</span>

## What are Transitionables?

Transitionables are nothing more than a tween, or a transition between two states over time.  Given two states and a duration, a Transitionable will create a smooth transition between its start and end state values for the length of the duration. We can use the data from Transitionables to create animations.

It is important to note that Transitionables do not update on their own, they instead calculate the state at the time `.get()` is called.

## Transitioning node components

You may have already encountered the API to several Famous components.

    var position = new Position(node);
    position.set(50, 50, 50, { duration: 3000 });


These components manage their internal state using Transitionables. The snippet above internally uses three Transitionables to tween the position component from its initial values `[0,0,0]` to the new values `[50,50,50]` over a 3 second period.


## Getting & Setting values

Set a Transitionable's initial state by passing a value to the constructor. Below, we create a new Transitionable instance and set its starting value to zero.

    var Transitionable = require('famous/transitions/Transitionable');
    var myValue = new Transitionable(0);

 Additionally, we can set the state of our transitionable at any time using the `.set()` method. Again, we set the state to zero in the snippet below.

    myValue.set(0);

Use the Transitionable's `.get()` method to retrieve its inner value at any time.

    var retrieved = myValue.get();
    retrieved === 0; // returns true

## Transitions

Create _transitions_ by passing the Transitionable's `.set()` method a new value and a duration. The duration tells the transitionable how long it should take to transition from the current value to the new value passed in. An on-completion callback may be optionally passed in as the third parameter.

    function done() { console.log('done'); }
    myValue.set(100, { duration: 3000 }, done);

The above code transitions from the transitionable's initial value of 0 to the new value of 100 in 3000 milliseconds (3 seconds). Note how we pass the duration in an options hash as the `.set()` method's second parameter.

What would you expect `.get()` to return here?

    var myValue = new Transitionable(0);
    myValue.set(100, { duration: 3000 });

    setTimeout(function() {
        return myValue.get()
    }, 1500);


Because the value is retrieved at 1500 milliseconds or halfway through the animation, the value retrieved from `.get()` should be 50.

## Sequences of transitions

The state for a chain of transitions can be set instantaneously using the Transitionable's `.from()` method, where transitions are subsequently queued using `.to()`. Internally, the `.set()` method uses `.from()` and `.to()`.

Additional methods include `.delay()`, which can be called to insert a delay in between transitions, and `.override()`, which is used to take over the current running transition.

    var myValue = new Transitionable();
    myValue.from(0).to(5, 'linear', 1000).delay(1000).to(-5, 'linear', 2000);

    setTimeout(function() {
        myValue.override(10, 'outQuad', 1000); // interrupts the transition to -5 midway to completion
    }, 3000);

Calling `.from()` during a transition will instantly cease the transition and empty the transition queue, while `.override()` will simply take over the current transition, leaving the queue intact.

## Halting and Pausing

The transition, and all further queued transitions, can be halted at any point by calling `.halt()` on the Transitionable. If instead you would like to pause a transition without canceling it entirely, use the `.pause()` and `.resume()` methods respectively. The Transitionable may be reset to the start of the current transition via `.reset()`.

    var myValue = new Transitionable(0);
    myValue.set(100, { duration: 3000 });

    myValue.halt(); // freeze at the current value and cancel future transitions

    myValue.set(-100, { duration: 3000 });
    myValue.pause(); // freeze, but retain future queued transitions
    myValue.resume(); // resume the transition queue
    myValue.reset(); // reset to the start of current transition and cancel future transitions

## Example: Fading out

Here's how we could use a Transitionable to make something fade out. We will set up a function to run every frame and query the transitionable's value.

    var node = context.addChild();
    var opacityTransitionable = new Transitionable(0);

    // Start transitionable
    opacityTransitionable.set(0, { duration: 5000 });

    var id = node.addComponent({
        onUpdate: function(time) {
            // Every frame, query transitionable state and set node opacity accordingly
            var newOpacity = opacityTransitionable.get();
            node.setOpacity(newOpacity);
            if (opacityTransitionable.isActive()) node.requestUpdate(id);
        }
    });

    node.requestUpdate(id);


We use the `.get` method to update the opacity on every frame. The snippet above would take 5 seconds for the linear fade-out animation to complete.

## Transitioning multiple values
And what if we want to transition an elements position? Position takes and x, y and z values; Do we need three separate transitionables? Luckily, no. The transitionable class lets us transition arrays.


    // set initial value to 0, 0, 0
    var myValue = new Transitionable([0, 0, 0]);

    // transition to new state of 100, 50, 10
    myValue.set([100, 50, 10], { duration: 3000 });


Each value of the array will be interpolated separately to its respective final value over the given duration. So if we called `.get()` halfway through the transition above we would receive an array with the values 50, 25 and 5.

Transitionables can also tween numeric properties of (nested) objects. For instance:

    var initial = {a: [0,0,0], b: {x: 100}, c: 0};
    var myValue = new Transitionable(initial);
    myValue.set({a: [100,200,300], b: {x: -100}}, {duration: 2000});

This will tween the `a` and `b` properties of the `initial` object to `[100,200,300]` and `{x: -100}` over 2 seconds, while leaving `c` untouched. Note: The initial object will be mutated in-place, so:
`myValue.get() === initial`.

## Easing Curves and Spherical Linear Interpolation

The specific easing curve for each transition can be specified a `curve` and passing in a function of `t`, where `t` ranges from 0 to 1, or passing a string and using one of our prebuilt easing curves.

    var myValue = new Transitionable(0);
    myValue.set(5, {duration: 2000, curve: function(t) { return t*t; }});
    myValue.set(10, {duration: 2000, curve: 'outBounce'});

For the specific case of quaternion interpolation, `method: 'slerp'` may be specified in the options hash to give a more natural rotational transition.

    var myValue = new Transitionable([1,0,0,0]);
    myValue.set([Math.cos(Math.PI/4),Math.sin(Math.PI/4),0,0], {duration: 1000, curve: 'outQuad', method: 'slerp'});


<div class="next-prev-container"><a rel="prev" class="previous" href="../pages/user-input.html">User Input </a><a rel="next" class="next" href="../pages/easing-curves.html">Easing Curves</a></div>

