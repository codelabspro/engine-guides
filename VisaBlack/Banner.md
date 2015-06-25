---
layout: default
title: The banner
---

<span class="intro-graf">We will build our banner from the `VisaAd` class in `VisaAd.js`. Although we already provided the code for this in the starter kit, we will introduce the View module and explain some of its helpful methods below.</span>

<div class="sidenote"><p>This section assumes prior knowledge of building simple Famous applications. If you are unfamiliar with the <a href="">scene graph</a> or <a href="">how to build a <i>'Hello World'</i> app</a>, check out our <a href="">guides</a> or <a href="">Carousel lesson</a>.</p></div>



<!-- We build the banner foundation with two files: `index.js` and `VisaAd.js`. If you open `index.js`, you will see the code below:

    var Context = require('famous-core').Context;
    var VisaAd = require('./VisaAd');
    //create new context
    var rootContext = new Context('body');
    //pass the first node into VisaAd
    var visaAd = new VisaAd(rootContext.addChild());

Note how we create the context externally and pass the root node to `VisaAd`. 

<div class="sidenote"><p>If you are unfamiliar with the <a href="">scene graph</a> or <a href="">nodes</a>, check out our <a href="">guides</a> or <a href="">Carousel lesson</a> for a deeper look into how we start an app.</p></div> -->

##Using our first View


If you open up `VisaAd.js`, you'll see that we've already created our first View. Instead of giving the View a [dispatch](), we give it the raw node and it does the rest behind the scenes. 

By calling `createHTMLElement()` and passing it an options parameter, we can create new [HTML renderable]() and style it using a minimal amount of code. In `VisaAd.js`, we use our first view to create the banner where all other elements will attach. 

    /*
    * VisaAd.js
    */

    var View = require('./View');

    function VisaAd (node, options) {

      this.node = node;
      this.view = new View(this.node);

      this.view.createHTMLElement({
        properties : {
          overflow : 'hidden',
          background: 'radial-gradient(ellipse at center, rgba(100,100,100,1) 0%,rgba(20,20,20,1) 100%)',
          perspective: '700px',
          '-webkit-perspective': '700px'
        }
      });

      this.view.setAbsoluteSize(300, 600, 0);
      this.view.setAlign(0.5, 0.5).setOrigin(0.5, 0.5).setMountPoint(0.5, 0.5);
    }

    module.exports = VisaAd;

Another useful View feature is the way we use the `setAbsoluteSize()`, `setAlign()`, `setOrigin()`, and `setMountPoint()`  methods above. When called on a View, these methods import their corresponding components and then set them to the values passed in. This saves a tremendous amount of overhead especially when creating multiple elements that need to be animated. Inspect `View.js` to see the full list of methods on `View`.

##See it in action

If you run the project ( `npm run start-dev` ), you should see the element below centered on the screen. Trace the app from `VisaAd.js` to the `index.js` file and from the `index.js` file to the output on the screen.

![textadded](./assets/images/banner.png)

Before moving on, make sure you understand how each command in `VisaAd` builds the element above. Reference our [guides]() on [components](), [positioning](), or [HTML renderables]() if you are stumped.

<div class='sidenote'>For embedding this ad in actual Web page, simply pass the Context (found in index.js) your target element and set the size of VisaAd's HTML element to <i>undefined</i>.</div>
  

<div class="sidenote--other">
<p><strong>Included file:</strong> <a href="https://github.com/Famous/lesson-visablack-steps/blob/step1/AddTimeline/src/start/VisaAd.js">VisaAd.js</a></p>
</div>

<div class="sidenote">
<p><strong>Section recap:</strong> <a href="https://github.com/Famous/lesson-visablack-steps/tree/step1/AddTimeline">Repo for this section</a></p>
</div>

<span class="cta">
[Up next: Text &raquo;](./Text.html)
</span>