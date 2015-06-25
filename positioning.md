---
layout: default
title: Positioning
---

<span class="intro-graf">In addition to rendering elements, scene graph nodes are also responsible for positioning.</span>

## Position

The `.setPosition()` method "translates" the node along its X, Y and Z axes by a certain
pixel value.

    node.setPosition(x, y, z);

Try changing the `.setAbsoluteSize()` and `.setPosition()` values below to see how it affects the node. Note how the node moves from the top left towards the bottom right. (Live coding will not work in mobile or smaller browsers).

<iframe src='https://staging.famous.org/examples/index.html?block=Positioning&detail=false&header=false' style="width:100%;height:600px; margin: 15px 0px 25px 0px" scrolling='no' class='code-block' allowtransparency='true'></iframe>


More specifically, `.setPosition()` defines a node's absolute offset relative to a point defined by its _Origin_, _Align_ and _Mount Point_.

## Align

Align positions a node within its parent's bounding box. The align defaults to `[0, 0, 0]`, which is the top left corner of the parent's front face, while `[1, 1, 0]` describes the parent's bottom right corner.

    node.setAlign(x, y, z);

Similar to position, set a node's alignment by calling the `.setAlign()` method directly on the node you wish to align. This will align the node directly and all child nodes below it.


Setting the align to `0.5, 0.5, 0.5` translates a node
to the center of its parent (50% of parent's height, width and depth). Modify the example below to see how this works. 

<iframe src='https://staging.famous.org/examples/index.html?block=Aligning&detail=false&header=false' style="width:100%;height:600px; margin: 15px 0px 25px 0px" scrolling='no' class='code-block' allowtransparency='true'></iframe>

It's important to note that since Align is relative to the parent context, its position will update if the parent's size changes. 

The example below demonstrates how we can recreate Align with a Position component (white square below). Note how the components below remain centered even if the size of your browser changes.

<iframe src='https://staging.famous.org/examples/index.html?block=AlignPosition&detail=false&header=false' style="width:100%;height:600px;margin: 15px 0px 25px 0px" scrolling='no' class='code-block' allowtransparency='true'></iframe>

The scene graph uses a three dimensional sizing model resulting in align ( and all positioning components ) accepting a third argument: `z`. For Align, it corresponds to the node's depth in relation to the parent context.

## Mount Point

The mount point defines a point (or anchor) within the node's bounding box where a linear
translation is applied.

    node.setMountPoint(x, y, z);

To center the node itself, you need to center its mount point via the `.setMountPoint()` method.

By default, mount point is set to `0,0,0` (top left corner of the node) and passing it
`0.5,0.5` moves the anchor to the center. Setting both align and mount point to
`0.5,0.5` will put the center of the node at the center of the parent context. Pay attention how mount point affects the three center aligned nodes below.

<iframe src='https://staging.famous.org/examples/index.html?block=MountPoint&detail=false&header=false' style="width:100%;height:600px;margin: 15px 0px 25px 0px" scrolling='no' class='code-block' allowtransparency='true'></iframe>

Try changing the mount point above to see how it affects the alignment.

## Origin

Origin is similar to mount point, but it sets the node's anchor point for
**rotation** and **scale**. The origin defines the point --on the node-- where it
should rotate around or scale from.  

    node.setOrigin(x, y, z);

Like Mount Point and Align, Origin accepts X, Y and Z percentage values as
decimal fractions. Origin is set in relation to the node's own bounding box (size).

For rotating a node around its center point like a pinwheel,
set its origin to `0.5, 0.5`, otherwise by default rotations (and scale) occur from the top left corner of the node.

<iframe src='https://staging.famous.org/examples/index.html?block=Origin&detail=false&header=false' style="width:100%;height:600px;margin: 15px 0px 25px 0px" scrolling='no' class='code-block' allowtransparency='true'></iframe>

How would you rotate a node around its top right corner? Try changing the values above to rotate the turquoise component by its top right corner: [1,0]. 

## Rotation

A node's rotation can either be specified in Euler angles or quaternions.

    node.setRotation(x, y, z); 
    // also  
    node.setRotation(x, y, z, w);

Constantly switching between Euler angles and quaternions on a frame-by-frame
basis results into computationally expensive conversions and should be avoided if
possible.


## Scale

Nodes can also be scaled. We recommend using scale for animating changes in size. Unlike the Size component, scale is hardware accelerated. 

    node.setScale(x, y, z);

The `x`, `y` and `z` values are multiplied against the current size of the node. Scaling a node affects its size and the size of all mounted child nodes. 


## Famous' Components

Famous ships with a set of commonly used components including:
 
  - `Origin`
  - `Align`
  - `MountPoint`
  - `Position` 
  - `Rotation`
  - `Scale`

You can use these components to easily animate a node's state on a frame-by-frame basis. See the FAQ section on a deeper explaination of [when to use a component](../pages/faq.html). 

Attach components by passing a node to the component's constructor.
    
    var Position = require('famous/components/Position');

    var node = new Node();
    var position = new Position(node);


Note: A `Size` component is also available, but should be avoided. We recommend using Scale for animations.

The example below illustrates how we use the `.setPosition()` method for setting a static state and a Position component for an animation. 

<iframe src='https://staging.famous.org/examples/index.html?block=PositionComponent&detail=false&header=false' style="width:100%;height:600px;margin: 15px 0px 25px 0px" scrolling='no' class='code-block' allowtransparency='true'></iframe>




See the [components section](./components.html) for a full list of available components. For a deeper look at animations, check out the [Transitionables section](./transitionables.html)



### Custom Components

Custom components can be used to enable more complex animations.

    var componentId = node.addComponent({
        node: null,
        transitionable: new Transitionable(0),
        onMount: function(node) {
            this.node = node;

            // Start the transition as soon as the node is being mounted.
            this.transitionable.set(Math.PI*2, { duration: 5000 });

            // Request the initial update.
            this.node.requestUpdate(componentId);
        },
        onUpdate: function() {
            this.node.setRotation(transitionable.get(), 0, 0);

            // Avoid updating the scene graph on a frame-by-frame basis by
            // explicitly requesting an update as long as the transition
            // (Transitionable) is active.
            if (this.transitionable.isActive())
                this.node.requestUpdate(componentId);
        }
    });
