---
layout: default
title: Sizing
---

<span class="intro-graf">To let developers fine-tune the size of elements, Famous introduces three size _modes_ that can be mix and matched to easily adjust the width (X), height (Y), and depth (Z) of nodes.</span>

## Adding Size

Instead of sizing elements directly, we size the _nodes_ that carry our elements. Every node contains an inner size component, but you can also attach a Size component as you would any other component.
    
    var someNode = root.addChild()
    // you can add a size component, but the
    // node already contains an inner size component
    var Size = require('famous/components/Size')
    var sizeComponent = new Size(someNode)

In this guide, we will show both the sizing methods on nodes and the sizing methods on the size component. However, for performance reasons, we recommend using the sizing methods on the nodes and using the _Scale_ component to animate size.

## Setting the Mode

Before setting the size you must first specify the _mode_ you are using the for X, Y, and Z dimensions. There are three size modes currently: Relative, Absolute, and Render. They may be set by using enumerations on the Node constructor or Size component, or by using strings. This is how you set the size mode.

    //set size mode on the node
    node.setSizeMode('absolute', Node.RELATIVE_SIZE, Size.RENDER);

    //set size mode on the component
    sizeComponent.setMode(Size.ABSOLUTE, 'relative', Node.RENDER_SIZE);

The `.setMode()` method accepts size modes for X, Y, and Z dimensions and defaults to relative when a dimension is left undefined. You can also pass a value of `Node.DEFAULT_SIZE` or `SizeComponent.DEFAULT` or `'default'` to access the default size.

## Absolute Size

Absolute size is perhaps the most familiar among web developers. Use the size component's `.setAbsolute()` method or the Node's `setAbsoluteSize()` method to set the exact pixel size of a node.

    // Node
    childNode.setSizeMode('absolute', 'absolute')
             .setAbsoluteSize(500, 500);

    // sizeComponent
    var sizeComponent = new Size(childNode)
    sizeComponent.setMode(Node.ABSOLUTE_SIZE, Node.ABSOLUTE_SIZE)
                 .setAbsolute(500, 500);

    //childNode will be 500px by 500px

_Note how we can chain these methods._

## Relative Size

The relative size mode combines several size values together to create a final size. The node has a _proportional_ size and a _differential_ size. Proportional size is multiplied against the size of the parent element, and differential size is added to it. Size is calculated in each dimension like so:

    parentSize * proportional + differential

Here is an example:

    var childNode = parent.setSizeMode('absolute', 'absolute')
                          .setAbsoluteSize(100, 100)
                          .addChild();

    // nodes are by default in relative mode
    childNode.setProportionalSize(0.5, 0.5)
             .setDifferentialSize(-10, -10);

childNode's size is now 40px in width and 40px in height. Here is another example using the size component:

    var parentSize = new Size(parent);
    parentSize.setMode('absolute', 'absolute').setAbsolute(100, 100);

    
    var childNode = parent.addChild();
    var childSize = new Size(childNode);

    childSize.setMode('absolute').setDifferential(20, 20).setProportional(2, 2).setAbsolute(10, 10);

childNode's size is now 10px wide and 220px high. Note how the y dimension is in 'relative' mode by default, but has been set to 'absolute' in x.

## Render Size

The Render size sets the size based on the node's renderable components. For example a _DOMElement_ will be automatically sized based on its HTML content. Simply set the size mode to 'render'.

    var size = new Size(childNode)
    .setMode(Node.RENDER_SIZE, Node.RENDER_SIZE, Node.RENDER_SIZE);

    childNode.setSizeMode('render', 'render');

## Combining Modes

Setting the size mode for width, height and depth gives the developer the option to mix different sizing styles and adjust elements more precisely. 

    var size = new Size(childNode)
    size.setMode(Node.PROPORTIONAL_SIZE, Node.RENDER_SIZE, Node.ABSOLUTE_SIZE)
        .setProportional(0.5, 0.5, 0.5)
        .setAbsolute(100, 100, 100)

In the above example:

* X size will be half of its parent's width (0.5).
* Y size will become the size of the element's content.
* Z size will be 100px exactly.

Modes can also be set using values: `0` corresponds to 'relative', `1` to 'absolute', and `2` to 'render'. Some of our examples may contain this notation since it is quicker to write. 
  
    // set to relative, absolute, and render for x,y,z
    node.setSizeMode(0,1,2)


## Window Size

For grabbing the `window` size, try to avoid pinging the DOM via `window.innerHeight` or `window.innerWidth`. Instead, give the root node a component with an `.onSizeChange()` method. 

    //add a size component
    var size = new Size(rootNode)

    size.onSizeChange = function(x,y,z){
       console.log("the parent's size is " + x + " " + y + " " + z)
    }

    //This simple 'custom component'
    //will do the same as above
    rootNode.addComponent({
      onSizeChange: function(x,y,z){
           console.log("the parent's size is " + x + " " + y + " " + z)
       }
    })


Every resize event (including the initial load) will trigger the function assigned to `onSizeChange` passing in the parent size as an argument. Above, we add the `.onsizeChange()` method to a Size component, but it can be added to any component even a custom one (second half of code snippet).

## Animating

In rare occasions, the size component can also be used to animate size.

    var size = new Size(rootNode);

    size.setMode('absolute', 'absolute')
        .setAbsolute(100, 100, 0, {duration: 1000});

  

This will animate the size of rootNode from (0, 0) to (100px, 100px) over one second. Note that Scale is recommended for these type of animations due to performance reasons.


