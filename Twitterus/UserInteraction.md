---
layout: default
title: User Interaction
---

<span class="intro-graf">Now that all of our elements are where we want them, let's make our Nav Buttons respond to clicks. We'll show you how to listen for DOM events. </span>

<!-- In Famous, handling traditional DOM events, such as `'click'`, `'mousedown'`, or `'touchstart'`, is relatively simple.  -->
Making your NavButton class listen to click events is as simple as calling:

    this.addUIEvent('click');


Add the line above to the bottom of `NavButton`'s constructor. 

<div class="sidenote--other"><p><strong>Modified files:</strong> <a href="https://github.com/Famous/lesson-twitterus-starter-kit/blob/step-6/src/twitterus/NavButton.js">NavButton.js</a></p></div>

Once NavButton has the 'click' event added to it, its DOMElement will submit the click event to the _scene graph_ whenever that DOMElement is clicked. What this means is that there is a click event that will begin at NavButton and bubble upwards. We will listen to this click event in `Twitterus` to affect the state of the application.

### Receiving Events

Events are received through a Node or Component's onReceive method. This method can be overwritten to employ custom logic when the event is received. Let's supply the following method in `Twitterus` to do just that.

    // Overwrite the onReceive method to intercept events flowing within 
    // the scene graph
    Twitterus.prototype.onReceive = function onReceive (event, payload) {

        // if the event is click then we know
        // that a NavButton was clicked
        // (NavButtons are the only element)
        // With the click event.
        if (event === 'click') {

            // get the id of the nav button
            var to = payload.node.getId();

            console.log(to);
        }
    };

Note that the payload has the node that emitted the UI event on it. By calling the NavButton's `.getId()` method we are able to get the id of the section that the NavButton points to. As an example we log the received id. Now that we are intercepting the click event in Twitterus we have to alert the rest of the application to change its state.

### Emitting events

Nodes have an emit method which broadcasts an event to its descendants in the scene graph. Let's emit an event, 'changeSection,' when the NavButton is clicked. Update `Twitterus.prototype.onReceive` with the following code:

    // Overwrite the onReceive method to intercept events flowing within 
    // the scene graph
    Twitterus.prototype.onReceive = function onReceive (event, payload) {

        // if the event is click then we know
        // that a NavButton was clicked
        // (NavButtons are the only element)
        // With the click event.
        if (event === 'click') {

            // get the id of the nav button
            var to = payload.node.getId();

            // emit the changeSection event to the subtree
            this.emit('changeSection', {
                from: this.currentSection,
                to: to
            });

            // set the current section
            this.currentSection = to;
        }
    };

Note how the twitterus class is now firmly acting as a controller for its subtree through event passing. Now that Twitterus is emitting the 'changeSection' event, we need to have the `Header`, `NavButton`s, and `Swapper` react to that event.



### Handling Events

First lets update our `Header` such that it'll respond properly to the 'changeSection' event. Exactly like how `Twitterus` responds to 'click,' we will provide the following `onReceive` method for the `Header` class.

    Header.prototype.onReceive = function onReceive (event, payload) {
        if (event === 'changeSection') {
            // we will uncomment this in the next section
            //this.changeSection(payload.to);
        }
    };

When the `Header` class receives the 'changeSection' event it will call its changeSection method. We will fill in this method in the next section.

Similarly we will fill create an onReceive method on both `Swapper` and `NavButton`.

    // overwrite onReceive to intercept events in the scene graph
    Swapper.prototype.onReceive = function onReceive (event, payload) {
        if (event === 'changeSection') {
            // we will uncomment this in the next section
            //this.changeSection(payload.to);
        }
    };

    // overwrite onReceive to respond to the changeSection event
    NavButton.prototype.onReceive = function onReceive (event, payload) {
        if (event === 'changeSection') {
            // swap on/off depend if this button points
            // to the apps current section
            if (payload.to === this.getId()) this.on();
            else this.off();
        }
    };

While the `Swapper.onReceive` delegates to `Swapper.changeSection`, `NavButton.onReceive` carries out the logic of calling `NavButton.on` if the app is swiping to the Section that this `NavButton` points to, or off otherwise.

###Event based programming

The Famous engine is set up for event based programming. The state of your application should be determined declaratively by the relationship between elements and their responses to events. You may have noticed that in all of our classes we haven't initialized them with any default state. The `Header` doesn't have a default Section title displayed, no `NavButton` has an 'on' or 'off' class applied, the position of every section is default. 

This is because at instantiation the only class that has information on what section is the app's current section is `Twitterus`. Because `Twitterus` is the controller of this application, it should be the single source of truth for what the state of the application is. This helps us structure our app and keep our code simple. No other class makes any assumptions and so they wait to hear a 'changeSection' event. 

We need `Twitterus` to propagate this information when the application is first started. In order to achieve this, we extend the `onMount` method of Node in `Twitterous`:

    // Overwrite on mount to emit the changeSection event the moment
    // twitter is added to the scene graph.
    Twitterus.prototype.onMount = function onMount (parent, id) {
       this.emit('changeSection', {from: null, to: this.currentSection});
    };

The `onMount` method is called when `Twitterus` is first added to the scene graph. Because `Node` is responsible for certain scene graph based logic when it is added, we must call `Node`'s `onMount` first, which we access from the class' `prototype`.

After `onMount` is called, the 'changeSection' event will have fired and every class in `Twitterus`' subtree will have responded, setting the initial state of the application.

## Seeing your progress

    $ famous deploy

If you run the famous deploy command from your terminal, you should now see your NavButtons respond to click events 

<div class="sidenote--other"><p><strong>Modified files:</strong> <a href="https://github.com/Famous/lesson-twitterus-starter-kit/blob/step-6/src/twitterus/Twitterus.js">Twitterus.js</a></p></div>

<div class="sidenote"><p><strong>Section recap:</strong> <a href="https://github.com/Famous/lesson-twitterus-starter-kit/tree/step-6">Code for this step</a></p></div>


<span class="cta">[Up Next: Animation &raquo;](./Animation.html)</span>




