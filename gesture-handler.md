---
layout: default
title: Gesture Handler
---

The `GestureHandler` is a component that makes it easy to set up listeners for mouse- and touch-based gestures.

    var context = new Famous.createContext('body');
    var node = context.addChild();


    var DOMElement = require('famous/dom-renderables/DOMElement');
    var element = new DOMElement(node);

    var GestureHandler = require('famous/components/GestureHandler');
    var gestures = new GestureHandler(node);
    function callback() { console.log('Gesture'); }
    gestures.on('drag', callback);

Currently supported gestures include `'drag'`, `'tap'`, `'rotate'`, and `'pinch'`. For touch devices, only up to the first two touches will be accounted for.

## Event Payload

Each event callback will receive an object detailing the status (`'start'`, `'move'`, or `'end'`) of the gesture, a timestamp, position and velocity information for the first two points of contact and for the center point, and optionally information regarding rotation and scale.

If listeners for `'rotate`' and/or `'pinch'` are specified, the payload will include `.rotation` and `.scale` properties describing the total rotation and scale since the start of the (two-finger) gesture, and delta properties describing the changes since that last time the event was processed. For two-finger gestures, position and velocity information regarding the midpoint of the two pointers will be contained in `.center`, `.centerDelta`, and `.centerVelocity`. For one-finger gestures, these properties will describe the first pointer.

Each payload additionally includes a `.points` property, describing the number of pointers involved in the gesture, and a `.current` property describing the number of pointer active immediately after the gesture. For example, in the case that a two-finger `'drag'` abruptly ends, leaving no pointers active on the screen, `.points` will be `2` while `.current` will be `0`.

One such payload is:

    {
        status: "move",
        time: 1432185853220,
        pointers: [
            {
                position: { x: 195, y: 148 },
                delta: { x: 0, y: -2 },
                velocity: { x: 0, y: -111.1111 }
            },
            {
                position: { x: 121, y: 281 },
                delta: { x: -2, y: 2 },
                velocity: { x: -111.1111, y: 111.1111 }
            }
        ],
        center: { x: 158, y: 214.5 },
        centerDelta: { x: -1, y: 0 },
        centerVelocity: { x: -55.5555, y: 0 },
        points: 2,
        current: 2,
        taps: 1,
        scale: 1.5261,
        scaleDelta: 0.0302,
        scaleVelocity: 1.6800,
        rotation: 0.0396,
        rotationDelta: -0.0013,
        rotationVelocity: -0.0741
    }

## Multi-Tap and Two-Finger-Tap

The payload above includes a `.taps` property which describes the number of consecutive taps, with each tap falling within a certain configurable threshold of the last. The `'tap'` event can be additionally configured to only fire on two-finger taps. To do so, instead of a event name string, one would pass in an options hash as the first parameter to `.on`:

    gestures.on({
        event: 'tap',
        points: 2,
        threshold: 100
    }, callback);

In the above, `callback` will only fire for two-finger taps, and `.taps` in the event payload will be incremented only if consecutive two-finger taps fall within 100ms of each other. By default, `.threshold` is 250ms.

To implement a double-tap handler, one might do the following in their callback:

    function processDoubleTap(e) {
        if (e.taps % 2 === 0) {
            // rest of callback
        }
    }
    gestures.on('tap', processDoubleTap);