---
layout: default
title: Components
---

<span class="intro-graf">
In the [scene graph section](scene-graph.html), we learned that elements in our application are represented structurally by _nodes_. Since nodes themselves aren't visible, we must decorate them with visual facets called _components_.
</span>

_Note: These concepts become more concrete in the [displaying content](displaying-content.html) guide._

## What is a component?

Components are encapsulated elements of behavior. They allow common functionality to be abstracted out of nodes, and extend nodes such that they are capable of executing the specific logic of your application. They can describe where an element is located on screen, how big it is, what content it has, and the style information it is associated with. All of the following are default components in Famous:

* `DOMElement` - for displaying HTML-formatted content
* `Mesh` - for displaying a WebGL mesh
* `Align` - anchor point in the element's context
* `MountPoint` - anchor point within the element
* `Camera` - perspective on the element
* `Opacity` - transparency of the element
* `Origin` - the point about which transforms act
* `Rotation` - the element's rotation transform in space
* `Scale` - the element's scale transform
* `Size` - the element's home size in pixels

## How to use components

Components are attached to the scene graph via nodes. Here's what this looks like in code:

    var scene = new Famous.createScene('body');
    var rootNode = scene.addChild(); 
    // set the size so the DOM element will be visible
    rootNode.setSizeMode('absolute', 'absolute', 'absolute')
            .setAbsoluteSize(250, 250)
    
    // add DOM element component with content
    var DOMElement = require('famous/dom-renderables/DOMElement');
    var element = new DOMElement(rootNode, {
          content: 'Hello Famous'
      });
    
    // add an align component so we can animate align
    // Use 'rootNode.setAlign(0.5,0.5)' for static alignment
    var Align = require('famous/components/Align');
    var alignComponent = new Align(rootNode);
    
    // move node to center over 1000ms period
    alignComponent.set(0.5,0.5, {duration:1000})

The node is passed to the component's constructor; the returned component instance exposes an API we can use to modify that component. Here, we add a `DOMElement` component with content and an Align component so we can animate the node as it moves to the center of the screen.

While we can add Position, Origin, Align, MountPoint, Rotation, and Scale components to nodes, it's recommended that you use the node's internal methods for setting static states (see [positioning](./positioning.html)). Attach components for frame-by-frame state transitions and avoid using the Size component whenever possible. 

<span class="art-insert" style="max-width:507px;max-height:352px">
![components](./assets/images/node-components.png)
</span>

Nodes may have zero or more components. Nodes without components might be used purely for grouping, for example. It's up to the developer to decide which components are appropriate to attach to which nodes in the scene graph.

### Custom Components

Writing custom components is one of the best ways to program in Famous. Components have the ability to respond to events and to schedule updates on the frame. To demonstrate, we will create a custom animation component.

A simple component can just be represented as an object literal. Here's an example:

    node.addComponent({
        onMount: function (node) {
            console.log('mounted at: ' + node.getLocation());
        },
        onTransformChange: function () {
            console.log('transform changed!');
        }
    });

This component will log the phrase 'mounted at: x', where x is the node's path in the scene graph, to the console every time it is added to the scene graph and log 'transform changed!' every time the node's transform changes.

Let's now make a component that logs the time every frame:

    node.addComponent({
        id: null,
        node: null,
        onMount: function (node) {
            this.id = node.addComponent(this);
            node.requestUpdate(this.id);
            this.node = node;
        },
        onUpdate: function (time) {
            console.log(time);
            this.node.requestUpdateOnNextTick(this.id);
        }
    });

This demonstrates an important concept in Famous: requesting updates. In order to execute logic on the frame, you first must request an update. In order to request updates your component will need to register itself with a node. So `onMount` we call node.addComponent to receive our id from the node and then call `node.requestUpdate` with the id that the node returned.

This schedules the components `onUpdate` method to be called on the next frame. In order to ensure that the `onUpdate` method is called every frame, we then call `node.requestUpdateOnNextTick` to schedule an update for the next frame. This will repeat forever, constantly logging the current time.

## Components and Transitionables

Transitionables provide a method to get a value over time. Here is a simple component class which drives an animation.

    // A component that will animate a node's position in x.
    function Animation (node) {
        // store a reference to the node
        this.node = node;
        // get an id from the node so that we can update
        this.id = node.addComponent(this);
        // create a new transitionable to drive the animation
        this.xPosition = new Transitionable(100);
    }

    Animation.prototype.start = function start () {
        // request an update to start the animation
        this.node.requestUpdate(this.id);
        // begin driving the animation
        this.xPosition.from(100).to(1000, {duration: 1000});
    };

    Animation.prototype.onUpdate = function onUpdate () {
        // while the transitionable is still transitioning
        // keep requesting updates
        if (this.xPosition.isActive()) {
            // set the position of the component's node
            // every frame to the value of the transitionable
            this.node.setPosition(this.xPosition.get());
            this.node.requestUpdateOnNextTick(this.id);
        }
    };

    var animation = new Animation(node);

    animation.start();

This component will animate the position along the x axis of this node from 100px to 1000px over a second whenever the components `start` method is called. The way that this behavior is abstracted from the node and the application shows the power of components. By creating custom components a developer can create a behavior that can be mixed into any node.

Next up, let's [put this into practice](displaying-content.md).
