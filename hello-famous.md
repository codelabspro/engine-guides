---
layout: default
title: Hello Famous
---

<span class="intro-graf">
The Famous platform is a high-performance rendering engine built in JavaScript. It gives you access to the DOM, WebGL, and 3D physics together via a powerful, low-level API. If you want to create stunning, the-future-is-now-style UIs that can deploy to almost any device, Famous is for you.
</span>

## A (simple) live example

Want to know what real Famous code looks like? Here's a simple demo that shows several of the key concepts in Famous working together. For illustration purposes, we've written this example to be straightforward at the expense of conciseness.

<iframe src="http://staging.famous.org/examples/?block=hello-famous&detail=false" class="famous-container"></iframe>

Anywhere you see an example like this -- note the Famous logo at the top, and the live demo at right -- you can modify it right here in your browser. Click in the code area to focus the code editor, and just start typing (note: the editor is disabled on mobile devices). You'll see your changes reflect as you type.

## Explanation

Let's take a closer look at what's going on in the example above. The code example begins with a few lines of required [boilerplate script](boilerplate.html) to initialize the Famous platform internals. You don't need to know exactly how the `FamousEngine` creates the rendering environment.

Next we create a _scene_, a special object that represents the home for all our visual elements. Think of it as the origin of space, from which all things will be positioned and drawn relative to.  Scenes are the root of the render tree and allows the ability to create Nodes that give shape to your application.

Finally, we attach [components](components.html) to the scene graph nodes in order to assign properties and produce visible output. The components in use here include `DOMElement` (for rendering HTML-formatted [content](displaying-content.html)), `Size` (for [sizing](sizing.html) elements), as well as `Align`, `MountPoint` and `Position` (for [positioning](positioning.html)).
