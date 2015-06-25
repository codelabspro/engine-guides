---
layout: default
title: Creating an App
---

<span class="intro-graf"> Meet the backbone of every Famous application: the Scene Graph. We will familiarize you with this structure as we build the foundation of our Twitterus app. </span>

## Scene Graph

Every Famo.us application attaches to the DOM through a [scene graph](#). The scene graph is a tree that holds together all of the elements of an app and expresses their relationship in space hierarchically. What this means is that when a parent is moved, its children move with it, but moving the children does not affect the position of the parent. This hierarchical structure promotes encapsulation for the elements of an application by allowing a developer to compose elements
out of subelements while at the same time reflecting their relationship in space.



<div class='sidenote'><p>For more on the <a href="">scene graph</a> check out our <a href="">guides</a>.</p></div>

## Nodes

The elements of the scene graph are called Nodes. The node class has three responsibilities: first, the node must represent a region in space, which means it must provide a position and a size; second, the node must maintain its relationships with its children and parent in the scene graph; and finally, the node must pass along information to its children and components.

In addition to these responsibilities, nodes have the opportunity to react to the events that drive the famous engine each frame. This provides one of two pathways for the developer to write custom logic in Famous, the other being components which will be explained below.

By extending Node, the developer can create a custom participant in the famous engine. In this example, we will extend node in several ways to create the visible elements of our application.

## Components

Components are similar to nodes except they do not participate in the scene graph. They are only attached to the graph by attaching themselves to certain nodes. While a node represents a monolithic element within your application, components represent small reusable behavior which can be used to extend a given node with additional functionality. Components are a great way to abstract common behavior out of individual nodes.

In this example, we will not be making a custom component, however we will be using some default components to render DOM elements and to transition the position of the various parts of our scene graph.

## Making a scene

Create a new scene graph -- or _context_ -- by calling `.createScene()` on the FamousEngine and passing in a selector to target. This selector tells Famous where to build a scene and append all the elements to the DOM. 


<img src="./assets/images/SG.png" alt="Scene Graph" style="max-width:400px">

Once the scene is built, we use `.addChild()` to extend the graph and add nodes. Calling `.addChild()` without an argument will result in a default Node being created and returned, but `.addChild()` can also take a specific node, which it will then append to itself. After a new node is constructed, we can then attach the visible elements of the application. 

In the image above, Twitterus (light grey circle) is the _root node_ of our application. It is added directly below the _Scene_. From this _root node_, the rest of our app will attach via a series of child and grandchild nodes. Let's see what the diagram above looks like written as code. 

Inspect the following lines in `index.js`.
    
    /*
    * index.js
    */

    var Twitterus = require('./Twitterus');
    var FamousEngine = require('famous/core/FamousEngine');
    // start the Engine
    FamousEngine.init();
    // create the app and pass in the target element
    var twitterus = FamousEngine.createScene().addChild(new Twitterus());

And the code below in `Twitterus.js`.

    /*
    * Twitterus.js
    */

    var Node = require('famous/core/Node');

    function Twitterus(mount) {
        // Extend Node
        Node.call(this);
    }

    // Extend the prototype
    Twitterus.prototype = Object.create(Node.prototype);

    module.exports = Twitterus;

<div class="sidenote--other"><p><strong>Listed files:</strong> <a href="https://github.com/Famous/lesson-twitterus-starter-kit/blob/step-1/src/twitterus/index.js">index.js</a> | <a href="https://github.com/Famous/lesson-twitterus-starter-kit/blob/step-1/src/twitterus/Twitterus.js">Twitterus.js</a> </p></div>


In `index.js`, we create the application by first instantiating a new Scene. `FamousEngine.createScene()` can take  a _CSS Selector_ as an argument, but when called without one it defaults to the body of your `index.html` file. Next, we use the `.addChild()` method to add the first node to the scene graph. The new twitterus instance we pass into `.addChild()` is simply a Node created from the `Twitterus.js` file. This will be the foundation of our app. 

<div class="sidenote"><p><b>Important:</b> Note how we create our application using Javascript's <a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Introduction_to_Object-Oriented_JavaScript#Custom_Objects">pseudo-classical</a> instantiation pattern. For the sake of brevity, we will simply refer to <code>Twitterus</code> and the rest of our <i>constructor functions</i> as classes. We leverage <i>classes</i> throughout Famous to keep our code modular.</p></div>     



<span class="cta">[Up Next: Architecture &raquo;](./Architecture.html)</span>

