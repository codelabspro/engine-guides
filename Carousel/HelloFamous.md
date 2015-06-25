---
layout: default
title: Hello Famous!
---

<span class="intro-graf">
Before we begin creating the carousel itself, let's display a basic "Hello World" message, just to establish the foundations for our application.
</span>

Throughout the app, we'll use the JavaScript "class" pattern to organize our code. It's a good idea to familiarize yourself with this pattern, since many Famous applications use it for structure.

## A basic structure

Copy and paste the code snippets below into the files `../index.js` and `Carousel.js`, which are included in the [carousel starter kit](https://github.com/famous/lesson-carousel-starter-kit) that you downloaded in the [getting started section](GettingStarted.html).
    
      /**
     * ../index.js
     */

    var FamousEngine = require('famous/core/FamousEngine');

    FamousEngine.init();

    // App Code
    var Carousel = require('./carousel/Carousel');
    var imageData = require('./data/data');
    var carousel = new Carousel('body', { pageData: imageData });

<br>

    /**
     * Carousel.js
     */

    var FamousEngine = require('famous/core/FamousEngine');
    var DOMElement = require('famous/dom-renderables/DOMElement');

    function Carousel(selector, data) {
        // Create a new Scene instance. Scenes are
        // the starting point for all Famous apps.
        this.context = FamousEngine.createScene(selector);

        // Add the first scene graph node to the
        // context. This is the 'root' node.
        this.root = this.context.addChild();

        // Decorate the node with a DOMElement
        // component, and use the component to apply
        // content and styling
        this.el = new DOMElement(this.root);
        this.el.setContent('Hello Famous!');
        this.el.setProperty('font-size', '40px');
        this.el.setProperty('color','white');
    }

    module.exports = Carousel;


## Explanation

In the code above, note how the carousel constructor calls `.createScene()` on the `Famous` component and passes it a _CSS selector_ for the specific DOM element (in this case, the `'body'`) that we want to mount our app to.

The scene object (returned from `.createScene`) forms the base of the Famous [scene graph](http://famous.org/learn/scene-graph.html) and handles attaching our app to the DOM. Adding child nodes to the scene -- extending the scene graph -- is the process by which we add visual elements to our app.

Pay attention to how the _root_ scene graph node gets passed as an argument to the DOMElement component's constructor. This is how we _decorate_ our scene graph nodes with visual components.

## See it Live

Now, let's see how easy it is to share and embed this project. Within your project's root directory execute the following command in your terminal:

    $ famous deploy

This command should return the sharable link and embeddable HTML code. If you vist the sharable link in the browser, you should see your full project served up by the Famous cloud services. 

Additionally, the embed code can now be embedded in any HTML page. Just make sure you give the `<div>` a size via CSS so it will display on your page.

## Next steps

Now that we understand how to put together a basic Famous scene and deploy our project, let's move on to architecting our more-complex carousel app.

<div class="sidenote--other">
<p><strong>Modified files:</strong> <a href="https://github.com/Famous/lesson-carousel-starter-kit/blob/step1-HelloFamous/src/index.js">../index.js</a> | <a href="https://github.com/famous/lesson-carousel-starter-kit/blob/step1-HelloFamous/src/carousel/Carousel.js">Carousel.js</a></p>
</div>

<div class="sidenote">
<p><strong>Section recap:</strong> <a href="https://github.com/famous/lesson-carousel-starter-kit/tree/step1-HelloFamous">Code for this step</a></p>
</div>

<span class="cta">
[Up next: Architecture &raquo;](./Architecture.html)
</span>
