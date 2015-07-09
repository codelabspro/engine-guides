---
layout: default
title: Program events
---


There have been some recent API changes and we are working to rewrite the guides here ASAP. 

To avoid this problem in the future, We are also working on versioned guides/docs. We apologize for the inconvenience. 


<!-- Program events are a way to broadcast events across all Nodes that are part of a
specific Scene. Event routing is within the problem domain of the "Dispatch".

Upon instantiation of a scene, a new Dispatch is being constructed. Subsequently
it is being used in order to route arbitrary events to specific Nodes ("UI Events")
or broadcast them globally ("Program events").

Contrasting those two concepts is inevitably in order to understand the use
cases, pros and cons of using either of those routing mechanisms.

## UI Events

UIEvents can be thought of as a generalized version of the DOM eventing model.
From a conceptual point of view, UIEvents are being fired in the Dispatch:

    var dispatch = myScene.getDispatch();
    dispatch.dispatchUIEvent(path, event, payload);


As one can see, dispatchUIEvent takes in three arguments:

  - `path` - Nodes can be uniquely defined by their location in the scene graph (often
being referred to as their id). A path is a stringified representation of
a Node's location. In order to get the path of a node, its `getPath()` method can used.
  - `event` - The type of the passed in event (e.g. "click").
  - `payload` - The event payload containing the data associated with the event. Needs to be
an object.

The Node which is associated with the specified path with receive the dispatched
event. It is important to realize that even though the specified node will
receive the event **first**, the event "bubbles" up (similar to the bubbling
phase in the DOM UI Eventing model).

In order to prevent this from happening, any node between the scene node and the
final, destination node has the ability to stop further bubbling by invoking
the event payload's `stopPropagation` method.

The event payload will be decorated with the `stopPropagation` method and the
`target` property. `payload.target` references the target node that has the
previously defined path. The `path -> Node` look up is being completed before the
event bubbling phase starts.

UIEvents need to be explicitly added to nodes. While this does not affect the
set of events that can be received by the Node, it provides a way for components
to act accordingly. E.g. the DOMElement uses its `onAddUIEvent` method to
send commands to the DOMRenderer to add the appropriate event listeners to the
context or the element in question.

## Receiving events

Both program and UI events are being subscribed to through the same mechanisms,
namely the receive pattern.

Whenever a node receives an event, its `onReceive` method is being invoked.

The most naive solution to subscribe for events is therefore to simply override
the Node's onReceive method:

    node.onReceive = function(event, payload) {
        console.log(
            'Received ' + event + ' event!'
        );

        // this.receive is an alias of the original Node#onReceive method.
        // It is equivalent to
        // Node.prototype.onReceive.call(this, event, payload).
        this.receive(event, payload);
    };

This pattern is perfectly valid and especially suited when extending Node to fit
specific use cases that are fundamental to the node itself as opposed to being
reusable behaviors.

Nevertheless, this pattern has a couple of disadvantages that make it harder to
maintain in more complex applications:

1.	Impossible to extract out common functionality of otherwise functionally independent nodes.
2.	Special casing node methods leads to a less declarative programming style and a scene graph that is harder to serialize.

Extracting out common event handling functionality through the use of custom
components solves those issues. Using custom components in Famous is easy and
encouraged. The above code can be factored out into a custom component
encapsulating the event logging into a kind of "black box".

    var myComponent = {
        onReceive: function(event, payload) {
            console.log(
                'Received ' + event + ' event!'
            );
        }
    };
    node.addComponent(myComponent);

While looking syntactically similar, the use of `addComponent` leads to a more
structured approach towards event handling, reason being that a node can have an
arbitrary number of components.

## Program Events

In contrast to UI events, program events are global. This means that they are
not associated with a specific Node, but are broadcasted across all nodes that
are part of the Scene that maintains the Dispatch.

    var dispatch = myScene.getDispatch();
    dispatch.dispatchEvent(event, payload);

This fundamental difference is reflected in the `dispatchEvent`'s' function
signature. It is equivalent to the one used by `dispatchUIEvent`, except that it
does not accept a path.

Main differences concerning the flow of events include:

1. UI Events simulate DOM-like **event bubbling**. As a direct consequence, the scene graph is being traversed **upwards**, which means that children can react to events previous to their ancestors. They can stop this using `stopPropagation`.
2. Program events **"flow down"**: The scene graph is being traversed using a **breadth first search**. Ancestors receive events before their children. There is no way to stop this process. The fact that there is no "final", target node differentiates this eventing model from the DOM's native capturing phase.

## Putting it all together

In the following example we create a very simple example showing off the
combination of program and UI events.

Basically, we're going to display two DOMElements, each of which is moving along
the x or y axis.

The values for their offsets is being determined by the user's mouse position.

Our Axis class inherits from Node. It has a DOMElement on it in order to display
the current offset. It also has a direction associated with it. The direction
can either be 'x' or 'y'.

    function Axis(direction) {
        Node.call(this);
        this._direction = direction;
        this._domElement = new DOMElement(this);
        this.setSizeMode('render', 'render');
    }

    Axis.prototype = Object.create(Node.prototype);
    Axis.prototype.constructor = Node;

The axis receives globally dispatched program events. It determines if it is
responsible for the event (e.g. Axis('x') exclusively handles x events) and
sets its offset and content accordingly.

    Axis.prototype.onReceive = function onReceive(type, offset) {
        if (type === this._direction) {
            if (this._direction === 'x') {
                this.setPosition(offset, 0);
            } else {
                this.setPosition(0, offset);
            }
            this._domElement.setContent(offset);
        }
        this.receive(type, offset);
    };

The background node's DOMElement is being used in order to listen for
`mousemove` events. It dispatches the locally received DOM UI event as a
program event using the node's emit method. `this.emit(event, payload)` is a
shorthand for
`scene.getDispatch().dispatchUIEvent(Node.getLocation(), ev, payload)`.

    function Background() {
        Node.call(this);
        this._domElement = new DOMElement(this, {
            properties: {
                background: '#eee'
            }
        });

        // addUIEvent is being used in order to instruct the node's DOMElement
        // to add the appropriate event listener through the DOMRenderer.
        // DOM events are being emitted as UI Events and routed accordingly.
        this.addUIEvent('mousemove');
    }

    Background.prototype = Object.create(Node.prototype);
    Background.prototype.constructor = Node;


    Background.prototype.onReceive = function onReceive(type, ev) {
        if (type === 'mousemove') {
            // dispatch globally
            this.emit('x', ev.x).emit('y', ev.y);
        }
        this.receive(type, ev);
    };

Finally, in order to set up the scene graph, we instantiate a new scene, add the
background and the two axes.

    var scene = FamousEngine.createScene();
    var root = scene.addChild(new Background());
    root.addChild(new Axis('x'));
    root.addChild(new Axis('y'));

In this example, we didn't use any custom components, but could have factored
out the `onReceive` methods into separate reusable components encapsulating
behaviors.
 -->
