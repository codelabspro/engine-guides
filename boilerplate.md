---
layout: default
title: Boilerplate
---

<span class="intro-graf">
All Famous applications require a few lines of boilerplate code in order to run without errors. Our engineers have tried to keep the boilerplate to a minimum while still allowing flexibility for developers who need it.
</span>

### Quick Start

After including the Famous rendering engine into your project, you will need to initialize the rendering engine and create a scene for your application.


    // Get the FamousEngine
    var FamousEngine = require('famous/core/FamousEngine');

    // Initialize the FamousEngine to start the rendering process
    FamousEngine.init();

    // Create a scene for the FamousEngine to render
    var scene = FamousEngine.createScene();

This is all you need to have the Famous Engine ready to start building your application.

### Using Web Workers

The Famous Engine was designed with a client/server architecture with the server being your application code and the client a thin rendering layer.  This abstraction allows the Famous Engine to run in web workers or even on a different machine leaving the UI thread open any other Javascript your application needs to execute.

To get this to run in a web worker some changes to the boilerplate code are needed.

Instead of initializing the FamousEngine, we will have one file for the rendering environment and one file for our scene.

#### worker.js

    var FamousEngine = require('famous/core/FamousEngine');
    var FamousEngine.createScene();

#### main.js

```javascript
var UIManager = require('famous/renderers/UIManager');
var Compositor = require('famous/renderers/Compositor');
var RequestAnimationFrameLoop = require('famous/render-loops/RequestAnimationFrameLoop');

var worker = new Worker('worker.js')
var uiManager = new UIManager(worker, new Compositor(), new RequestAnimationFrameLoop());
```

If you look at the [worker branch][1] in our seed project, you can see this feature implemented.

While the performance benefits of using web workers in your application today is highly dependent on what else is happening on the page, web workers will only continue to improve and having an architecture designed to utilize their strengths allows an exploration into more complex web application design patterns.

[1]: https://github.com/Famous/engine-seed/tree/worker

