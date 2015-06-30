---
layout: default
title: User Interaction
---

<span class="intro-graf">
We've nearly got a working carousel, but we're still missing one important thing: user interaction. In Famous, we can accept and handle input by listening to traditional DOM events.
</span>

Lets start by making arrow interactions move the anchor positions of the page. Since our boxes are attached to the anchors by springs, the movement will provide the forces needed to animate the carousel.

## Emitting events

Within our classes, we'll set things up such that DOM events will be broadcasted up to the parent. We can accomplish this by using the Node's `.emit()` method.

Open the `Arrow.js` and add the following:


    // Listen to the click event
    this.addUIEvent('click');


Notice how we would add the event to the node with `.addUIEvent()`, and then listen to DOM events on the `DOMElement`. This asks the node to listen for an event, which it would then pass along to each of its components.

<div class="sidenote">
<p><strong>Modified files:</strong> <a href="https://github.com/famous/lesson-carousel-starter-kit/blob/step8-EmittingHandlingEvents/src/carousel/Arrow.js">Arrow.js</a></p>
</div>

## Handling events

In the `Carousel.js` file, let's create an event handler to listen for events. We'll also write a private `_bindEvents` function to act upon these events.

Within the `Carousel` constructor function, add the following lines:

    this.currentIndex = 0;
    _bindEvents.call(this);

Next, add the following `_bindEvents` function to the bottom of the same file.

    function _bindEvents() {
        this.root.addComponent({
            onReceive: function(e, payload) {
                // Verify the event as being 'click' and the appropriate 'Node'
                var isArrowClicked = (e === 'click') &&
                                     (payload.node.constructor === Arrow);
                if (isArrowClicked) {
                    var direction = payload.node.direction;
                    var amount = payload.node.amount;
                    amount = amount || 1;

                    var oldIndex = this.currentIndex;

                    var i = oldIndex + (direction * amount);
                    var min = 0;
                    var max = this.pageData.length - 1;

                    var newIndex = i > max ? max : i < min ? min : i;

                    if (this.currentIndex !== newIndex) {
                        this.currentIndex = newIndex;
                        this.dots.pageChange(oldIndex, this.currentIndex);
                        this.pager.pageChange(oldIndex, this.currentIndex);
                    }
                }
            }.bind(this)
        });
    }


When receiving a `'click'` event, we check to see if it's from the correct source; if so, the carousel instance will call the `pageChange` method on the `Dots` and `Pager` instances. You'll notice that the `Dots` instance already has a `pageChange` method, but we haven't yet added one in Pager. Let's build one now:

    /**
     * Pager.js
     */

    Pager.prototype.pageChange = function(oldIndex, newIndex) {
        if (oldIndex < newIndex) {
            this.pages[oldIndex].anchor.set(-1, 0, 0);
            this.pages[oldIndex].quaternion.fromEuler(0, Math.PI/2, 0);
            this.pages[newIndex].anchor.set(0, 0, 0);
            this.pages[newIndex].quaternion.set(1, 0, 0, 0);
        } else {
            this.pages[oldIndex].anchor.set(1, 0, 0);
            this.pages[oldIndex].quaternion.fromEuler(0, -Math.PI/2, 0);
            this.pages[newIndex].anchor.set(0, 0, 0);
            this.pages[newIndex].quaternion.set(1, 0, 0, 0);
        }
        this.currentIndex = newIndex;
    };

We move our physics anchors off screen by changing their x-positions to `-1` or `1`. Since we multiply our anchor position by the width of the screen in `Pager.update()`, the anchors will be offset fully from the center. If you save, run `$ famous deploy`, and visit the sharable link, the app should now animate when you click or touch one of the arrows.

<span class="sidenote">Modified Files: [Carousel.js](https://github.com/famous/lesson-carousel-starter-kit/blob/step8-EmittingHandlingEvents/src/carousel/Carousel.js)
  | [Pager.js](https://github.com/famous/lesson-carousel-starter-kit/blob/step8-EmittingHandlingEvents/src/carousel/Pager.js)
</span>


<div class="sidenote--other">
<p><strong>Modified files:</strong> <a href="https://github.com/Famous/lesson-carousel-starter-kit/blob/step8-EmittingHandlingEvents/src/carousel/Pager.js">Pager.js</a></p>
</div>

<div class="sidenote">
<p><strong>Section recap:</strong> <a href="https://github.com/Famous/lesson-carousel-starter-kit/tree/step8-EmittingHandlingEvents">Code for this step</a></p>
</div>

<span class="cta">
[Finish &raquo;](./Finish.html)
</span>
