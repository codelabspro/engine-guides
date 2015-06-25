---
layout: default
title: Organizing our Scene
---

## Creating view classes

There are many ways to structure your Famous code.  In this lesson we'll use simple classes that receive and decorate a node.

Here's an example of an empty view class.

    function MyView(node, options) {
        // decorate the node here
    }


So before we start crafting our laptop, we should take a minute to lay out the views we are going to use.

Let's start by creating three new files:

`DeviceView.js`
`ScreenView.js`
`OBJView.js`

The DeviceView is our top level view, this will create our OBJView our ScreenView.

    var ScreenView = require('./ScreenView');
    var OBJView = require('./OBJView');

    function DeviceView(node, options) {

        // Add screenNode and screenView

        this.screenNode = node.addChild();
            .setSizeMode(1, 1, 1)
            .setAbsoluteSize(200, 200, 200);

        this.screenView = new ScreenView(this.screenNode);

        // Add our objNode and objView

        this.objNode = node.addChild()
            .setPosition(0, 200, 0)
            .setSizeMode(1, 1, 1)
            .setAbsoluteSize(200, 200, 200);

        this.objView = new OBJView(this.objNode);
    }

    module.exports = DeviceView;


Here we are requiring our other views and passing them child nodes of the DeviceView.  For this step we will size the nodes and position them so that our child views are not in the same place.

For each of the child views we will simply add DOMElements to help us visualize their position.

    function ScreenView(node, options) {
        this.element = new DOMElement(node)
            .setProperty('background-color', 'white')
            .setContent('This is the ScreenView!');
    }



    function OBJView(node, options) {
        this.element = new DOMElement(node)
            .setProperty('background-color', 'pink')
            .setContent('This is the OBJView!');
    }

Finally we should add our DeviceView to the scene.  So back in `App.js`, instead of adding the Mesh and DOMElement directly to our scene, we'll add the DeviceView.


    // Add the device view to our scene.

    var deviceNode = scene.addChild()
        .setOrigin(0.5, 0.5, 0.5)
        .setAlign(0.5, 0.5, 0.5)
        .setMountPoint(0.5, 0.5, 0.5)
        .setSizeMode(1, 1, 1)
        .setAbsoluteSize(200, 200, 200);

    var deviceView = new DeviceView(deviceNode, {});


Now when we run our app we should see the two subviews, OBJView and ScreenView spinning around in unison!  You can imagine now how useful this view nesting will be when we want our laptop screen to rotate in sync with our laptop body!

<span class="cta">[Up Next: Importing the Model &raquo;](./ImportingTheModel.html)</span>
