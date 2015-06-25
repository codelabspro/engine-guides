---
layout: default
title: WebGL Content
---


WebGL is a Javascript API for creating 3D graphics through the DOM's `<canvas>` element. Famous streamlines the API and pairs it with DOM making it easy to animate 3D objects (Meshes) and integrate stunning graphics into web applications. 

## Mixed Mode & Meshes

In WebGL, a Mesh is any object drawn to the canvas element. Famous' _Mesh component_ can be thought of as the GL equivalent of the [DOM element component](./displaying-content.html). Unique to Famous is the ability to leverage both DOM and GL using the same coordinate system. Through the scene graph and a single context sized canvas element, Famous plots Meshes on the canvas in line with the rest of your app.

We can add a Mesh component to any node including nodes with DOM element components.

    var Mesh = require('famous/webgl-renderables/Mesh')
    var node = scene.addChild()
    var element = new DOMElement(node)
    // Nodes can contain both a DOM element and GL Mesh
    var mesh = new Mesh(node); 

Once attached to a node, DOM elements and GL meshes can be seamlessly positioned and animated in unison. 

##Adding shape

Every Mesh is drawn based on a set of points or vertices collectively know as a _Geometry_. Geometries give meshes their shape and without a Geometry a mesh is not visible.

We define Geometries with the `.setGeometry()` method.
    
    var Mesh = require('famous/webgl-renderables/Mesh')
    
    var mesh = new Mesh(node); 
    mesh.setGeometry('Sphere');

The code above gives the Geometry a spherical shape. The `.setGeometry()` method also accepts a second options parameter.  

    mesh.setGeometry('Sphere', { detail: 100 });

Providing the options parameter with a `detail` property determines the amount of verticies to use when drawing a Geometry. 

 <span class="sidenote"><b>Note:</b> The detail option becomes important for managing performance or increasing detail when drawing shapes with curves.</span>

## Primitives

Famous gives us a number of pre-made Geometries called Primitives. Primitives range from simple shapes like the Sphere above to more complex ones including Icosahedrons and Parametric cones.

Currently, Famou.s give us the following Primitives:

  - Box
  - Circle
  - Cylinder
  - Geodesic Sphere
  - Icosahedron
  - Parametric Cone
  - Plane
  - Sphere
  - Tetrahedron
  - Torus
  - Triangle

## Custom Geometries

Developers familiar with vertices, and some GL basics, can also create custom Geometries. Below is a custom Geometry constructor for a triangle.
    
    var Geometry = require('famous/webgl-geometries/Geometry')

    function CustomTriangle(options) {
        Geometry.call(this,
            { buffers: [
                { name: 'pos', data: [-1, 1, 0, 0, -1, 0, 1, 1, 0], size: 3 },
                { name: 'normals', data: [0, 0, 1, 0, 0, 1, 0, 0, 1], size: 3 },
                { name: 'texCoord', data: [0.0, 0.0, 0.5, 1.0, 1.0, 0.0], size: 2 },
                { name: 'indices', data: [0, 1, 2], size: 1 }
            ]}
        );
    }

We can use the custom Geometry above by passing it to the `.setGeometry()` method as we would any other Primitive.
    
    var geometry = new CustomTriangle();
    var mesh = new Mesh();
    mesh.setGeometry(geometry);

Above, we create a new custom Geometry instance and pass it to a Mesh. 

## GL drawing primitives

Advanced GL developers may want to change the GL drawing primitives (not to be confused with Geometry Primitives). To do this, pass a `type` option to the Geometry constructor with the primitive type listed as a string.
    
    var Geometry = require('famous/webgl-geometries/Geometry');

    new Geometry({ type: 'LINES' });
    new Geometry({ type: 'TRIANGLES' });
    new Geometry({ type: 'POINTS' });

## Dynamic Geometry

A dynamic Geometry is different from a static Geometry in that its buffers (verticies, normals, etc.) can change value at any time. There are performance reasons for this distinction. Dynamic Geometries are useful for animating the vertices of a mesh i.e., morph target animation. 

To change whether a Geometry is dynamic, pass the the Geometry constructor the `dynamic` option with its value set to true. 
    
    var Geometry = require('famous/webgl-geometries/Geometry');

    new Geometry({ dynamic: true });


