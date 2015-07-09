---
layout: default
title: User input
---

There have been some recent API changes and we are working to rewrite the guides here ASAP. 

To avoid this problem in the future, We are also working on versioned guides/docs. We apologize for the inconvenience. 



<!-- Handling user input is a central component of every non-trivial application.
Building applications in Famous is no exception.

User Input in Famous is being handled through DOM events. The general pattern
for listening to DOM events can be summarized as follows:

  - Assuming you want to listen for e.g. `click` events on a specific node,
    the node has to have a DOMElement attached to it.
  - The DOMElement needs to be instructed to add an appropriate event listener. This is being achieved by calling `node.addUIEvent('click')`.
  - Receive the events. The events are now being dispatched on the previously defined node and all its ancestors ("event bubbling"). You can either override the Node's `.onReceive()` method or add individual components. Adding custom components usually leads to more modularized and reusable code.

For a more detailed introduction to the Eventing System, see the introduction to [Program Events](program-events.html).

## Example

In the following example we are going to create a simple Hello World
Application demonstrating how to read from an `<input>` element and broadcast
the value changes of the form element.

    function NameInput() {
        Node.call(this);
        this._domElement = new DOMElement(this, {
            tagName: 'input',
            attributes: {
                placeholder: 'Your Name'
            }
        });
        this.setProportionalSize(1, 0.5);

        // Adding keyup as an UI Event allows the previously added DOMElement to
        // instruct the DOMRenderer to act accordingly.
        this.addUIEvent('keyup');
    }

    NameInput.prototype = Object.create(Node.prototype);
    NameInput.prototype.constructor = NameInput;

In order to receive the `keyup` DOM event, we override the Node's `onReceive`
method. We add the `keyup` UI event in the constructor. Adding a UI Event
to a node notifies all components that are currently attached to it.

The DOMElement, being a component, instructed the DOMRenderer to attach an
appropriate event listener to the respective element in the DOM. All subsequent
`keyup` events that are being emitted on this element are being forwarded
to the NameInput node through the Dispatch.

    NameInput.prototype.onReceive = function onReceive(type, ev) {
        // Check if the received event is the keyup event.
        if (type === 'keyup') {
            // Instead of reading the value from the DOMElement, we have access
            // to the DOMElement's current value through the event payload
            // itself.
            //
            // We globally emit (broadcast) the event to all nodes (the entire
            // scene graph). This means all nodes, including this node, will
            // receive the dispatched event.
            this.emit('name', ev.value);
        }
        // Equivalent to Node.prototype.receive.call(this, type, ev);
        // Used in order to allow potential components to receive the event.
        this.receive(type, ev);
    };

In order to display the input changes, we create another Node in order to
listen for `name` events and display them in an appropriate way.

We call this node "NameOutput".

    function NameOutput() {
        Node.call(this);
        this._domElement = new DOMElement(this);

        this
            .setProportionalSize(1, 0.5)
            .setAlign(0, 1)
            .setMountPoint(0, 1);
    }

    NameOutput.prototype = Object.create(Node.prototype);
    NameOutput.prototype.constructor = NameOutput;

The constructor function simply positions and sizes the DOMElement correctly to
be displayed underneath the NameInput.

    NameOutput.prototype.onReceive = function onReceive(type, ev) {
        if (type === 'name') {
            this._domElement.setContent('Hello, ' + ev + ' !');
        }
        this.receive(type, ev);
    };

In the NameOutput's `onReceive` method we listen for the globally dispatched
`name` event and set the event's payload as content.

Finally, we set up our scene graph:

    var scene = FamousEngine.createScene();
    scene.addChild(new NameInput());
    scene.addChild(new NameOutput());
 -->
