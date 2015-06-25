---
layout: default
title: Animation
---

<span class="intro-graf">Now that the elements of our app are all neatly organized using the Famous _scene graph_, you'll see just how easy it is to add animations. We'll show you how to create simple transitions by modifying the components in our app. </span>

We want Twitterus to animate our header and swapper when the buttons are clicked. Let's set up `'changeSection'` methods on our `Header` and `Swapper` classes to do just that.

## Header

When a button is clicked, we want to move the current title off the screen, switch it out for a new one, and then move it back down to its original position. We'll do this by calling the `.set()` method on the title's align component.

In addition to X,Y, and Z values, the `.set()` method also takes `options` and _callback_ parameters. We use the `options` to introduce transitions and the _callback_ to trigger actions once those animations are finished. Let's see how this works below.

    Header.prototype.changeSection = function changeSection (to) {
        // -1 in Y will put the title directly above its parent
        this.titleAlign.set(0, -1, 0, {duration: 250}, function () {
            // while the title is off screen
            // change the content
            this.titleEl.setContent(to);

            // align 0, 0, 0 places the title back into its parent
            // exactly
            this.titleAlign.set(0, 0, 0, {duration: 250});
        }.bind(this));
    };

Above we call the `.set()` method two times: once initially and again within the _callback_ function. Note how the `options` parameter passed to `.set()` includes a `duration` property. This specifies how long it should take to transition from the current values to the new values passed in. Because no curve is specified, the default curve is chosen which is _'linear'_. The linear curve creates an even amount of _tween_ frames in between start and end values.

<div class="sidenote--other"><p><strong>Modified files:</strong> <a href="https://github.com/Famous/lesson-twitterus-starter-kit/blob/step-7/src/twitterus/Header.js">Header.js</a></p></div>

## Swapper

Let's make our `Swapper` _swap_ out sections similar to the Title above. Open up `Swapper.js` and add the following method.

    Swapper.prototype.changeSection = function changeSection (to) {
        // Swap out any section that isn't the new section
        // and swap in the new section
        data.sections.forEach(function (section) {
            if (section.id === to) 
                // 500 millisecond transition
                this.sections[section.id].align.set(0, 0, 0, {
                    duration: 500
                });
            else
                // 1 in x will put the top left corner of the 
                // section directly off the screen
                this.sections[section.id].align.set(1, 0, 0, {
                    duration: 500
                });
        }.bind(this));

        this.currentSection = to;
    };

Next, we just need to uncomment the calls to these functions in Swapper and Header classes. 
        
        /**
        * Swapper.js
        */

        Swapper.prototype.onReceive = function onReceive (event, payload) {
            if (event === 'changeSection') {
                this.changeSection(payload.to);
            }
        };

<br>

        /**
        * Header.js
        */

        Header.prototype.onReceive = function onReceive (event, payload) {
            if (event === 'changeSection') {
                this.changeSection(payload.to);
            }
        };

## Ready to Share

    $ famous deploy

After saving, run the famous deploy command in your terminal to get your shareable link and embed code for the finished project. Now you can visit the page or embed this project to an existing website!


<div class="sidenote--other"><p><strong>Modified files:</strong> <a href="https://github.com/Famous/lesson-twitterus-starter-kit/blob/step-7/src/twitterus/Swapper.js">Swapper.js</a></p></div>

<div class="sidenote"><p><strong>Section recap:</strong> <a href="https://github.com/Famous/lesson-twitterus-starter-kit/tree/step-7">Code for this step</a></p></div>


<span class="cta">[Up Next: Finish &raquo;](./Finish.html)</span>