---
layout: default
title: Mixing Modes
---

## DOM meets WebGL

Let's get straight to it.  How do we 'mix modes'?

First let's change our 'Box' geometry to a 'Sphere' to better visualize this mixed-mode-magic^tm.

	var mesh = new Mesh(meshNode)
	    .setGeometry('Sphere');


Now lets add a div to our scene.


	var element = new DOMElement(meshNode)
	    .setProperty('background-color', 'pink');


Congratulations! You have mixed modes!  You should see a DOM 'div' element intersecting a WebGL sphere.

## Explanation

You might have noticed that we used the same node for our __Mesh__ as our __DOMElement__.  This means both of these components have the same and position.  You can think of size in Famous as a bounding box for both DOMElements and WebGL shapes.  The sphere perfectly fits within the DOMElement here because it shares the same bounding box.

Switching back to a Box geometry helps illustrate this point.

	var mesh = new Mesh(meshNode)
	    .setGeometry('Box');


Doing this actually completely obscures the DOMElement.  Because they share the same bounding box, and the Box completely fills this bounding box, the DOMElement cannot be seen.

Try experimenting with other geometries like `Torus` and `Tetrahedron` to see other geometries fill this bounding box.

<span class="cta">[Up Next: Organizing Our Scene &raquo;](./OrganizingOurScene.html)</span>