---
layout: default
title: User Interaction
---

<span class="intro-graf">To make our banner interactive, we will trigger a secondary animation when you swipe the card. This animation will spin the card in a specific direction and then reveal a random message on the banner.</span>

### Adding elements
Similar to our intro animation, we will create all of the elements for the Card Swipe section first using a `_createCardSwipe()` method. We will also need a call to it within our VisaAd class. 
    
    //add this below the other method calls in VisaAd
    this._createCardSwipe();

Next let's create the elements using the View module once again.

    VisaAd.prototype._createCardSwipe = function() {
        this.clickToWinText = new View(this.node.addChild());
        this.clickToWinText.createHTMLElement({
            content: 'SWIPE CARD FOR A CHANCE TO WIN $1,000',
            properties: {
                color: 'white',
                textAlign: 'center',
                fontFamily: 'Lato, sans-serif',
                fontSize: '19px',
                fontWeight: '600',
                border: '1px solid #ffffff',
                boxSizing: 'border-box',
                padding: '5px 5px'
            }
        });

        this.clickToWinText.setAbsoluteSize(235, 60, 0);
        this.clickToWinText.setAlign(0.5, 0.75).setMountPoint(0.5, 0.5);

        this.tryAgainText = new View(this.node.addChild());
        this.tryAgainText.createHTMLElement({
            content: 'APPLY NOW FOR EXTRA REWARDS',
            properties: {
                color: 'white',
                textAlign: 'center',
                fontFamily: 'Lato, sans-serif',
                fontSize: '19px',
                fontWeight: '600',
                border: '1px solid #ffffff',
                boxSizing: 'border-box',
                padding: '5px 5px'
            }
        });

        this.tryAgainText.setAbsoluteSize(235, 60, 0);
        this.tryAgainText.setAlign(0.5, 0.75).setMountPoint(0.5, 0.5);
        this.tryAgainText.setOpacity(0);

        this.winnerText = new View(this.node.addChild());
        this.winnerText.createHTMLElement({
            content: 'CONGRATULATIONS! YOU HAVE WON $1,000',
            properties: {
                color: 'white',
                textAlign: 'center',
                fontFamily: 'Lato, sans-serif',
                fontSize: '19px',
                fontWeight: '600',
                border: '1px solid #ffffff',
                boxSizing: 'border-box',
                padding: '5px 5px'
            }
        });

        this.winnerText.setAbsoluteSize(235, 60, 0);
        this.winnerText.setAlign(0.5, 0.75).setMountPoint(0.5, 0.5);
        this.winnerText.setOpacity(0);

        this.winnerTitle = new View(this.card.addChild());
        this.winnerTitle.createHTMLElement({
            content: 'W I N N E R',
            properties: {
                color: 'white',
                textAlign: 'center',
                fontFamily: 'Lato, sans-serif',
                fontSize: '17px',
                fontWeight: '600',
            }
        });

        this.winnerTitle.setAbsoluteSize(235, 50, 0);
        this.winnerTitle.setAlign(0.5, 0.53).setMountPoint(0.5, 0.5);
        this.winnerTitle.setOpacity(0);

        this.didSwipe = 0;
    }

Note how we add the winner to the card view. This will allow us to position the WINNER message in the center of the card.

### Gesture Handler

Now that we have the elements for swipe messages, we will need to listen for swipes using  a [GestureHandler](). Add this line to the top of `VisaAd.js` to import your handler.

    var GestureHandler = require('famous-components/src/GestureHandler');

Using this module we can create a gesture handler that will respond to drag events. We want this handler to start listening for swipes only after the main intro animation has finished. To do this, we will add it through a callback passed to the main Timeline. 

Let's modify our Timeline's `.set()` method at the bottom of `_initiateAnimations()` so it resembles the code below:

    this.timeline.set(12000, {duration: 12000}, function() {
            this.card.gesture = new GestureHandler(this.node.getDispatch(), [
                {
                    event: 'drag',
                    callback: this._clickToWin.bind(this)
                }
            ]);
            this.eventTime = 0;
        }.bind(this));

The callback above will only add a gesture handler to the Card after our main timeline has finished. Now we need to define a `_clickToWin()` function so the code above will have something to call on a `'drag'` event. 

### Swiping logic

Add the following code to create a `_clickToWin()` method. This will determine if a swipe is valid and trigger an event if so.


    VisaAd.prototype._clickToWin = function(event) {
    var direction = 0;
    if (!this.didSwipe) {
        if (event.time > this.eventTime) {
            if (event.centerVelocity.x > 250) {
                this.eventTime = event.time + 500;
                direction = 1;
                this._spinCard(direction);
                this.didSwipe = 1;
            }
            if (event.centerVelocity.x < -250) {
                this.eventTime = event.time + 500;
                direction = -1;
                this._spinCard(direction);
                this.didSwipe = 1;
            }
        }
    }
    }

Above, the `this.eventTime` value limits swipes to every 500ms. A swipe is only valid if the velocity is greater than the 250 threshold and it hasn't yet been swiped. If a swipe is valid, we will trigger a `_spinCard()` call and pass it the direction of the swipe as a 1 or -1 value.


Now we will need a `_spinCard()` call.

The spin card event will take a direction (1 or -1) and use this data to spin the card. Additionally this method will randomly hide or show "Winner" or "Apply Now" messages. These events will all need to be animated.

## Swipe animations

Since the swipe events will contain multiple animations, we will use another Timeline instance. Add one just below our other timeline in the `VisaAd` class.  
    
    this.clickTimeline = new Timeline({
            timescale : 1
    });

Next, we will register the animations with this timeline. The code below will animate the card. Notice how we use the direction passed into `_spinCard()` to modify the rotation animation.


    VisaAd.prototype._spinCard = function(direction) {
        this.clickTimeline.registerComponent({
            component: this.card.validateRotation(),
            path: [
                [0, [0, 0, 0]],
                [2000, [0, Math.PI * 20 * direction, 0], Easing.getCurve('easeInOut')]
            ]
        });

       
        
        //we will add the "Winner" and "Apply Now" animations here 


    }


When we run the `_spinCard()` animation, we want to it randomly reveal one of two messages. Add the code below to do just that:
        
        //this will hide the initial call to action
        this.clickTimeline.registerComponent({
            component: this.clickToWinText.validateOpacity(),
            path: [
                [0, 1],
                [2000, 0]
            ]
        });
        
        // randomly give us a 1 or 0 value for `winner`
        var winner = 0;
        if (Math.floor(Math.random()*2) === 0) {
            winner = 1;
        }
        

        this.clickTimeline.registerComponent({
            component: this.winnerText.validateOpacity(),
            path: [
                [0, 0],
                [2000, 0],
                [3000, winner]
            ]
        });
        
        //have WINNER appear to be on the card by fading opacity
        this.clickTimeline.registerComponent({
            component: this.winnerTitle.validateOpacity(),
            path: [
                [0, 0],
                [1000, 0],
                [2000, winner * 0.75]
            ]
        });

        this.clickTimeline.registerComponent({
            component: this.tryAgainText.validateOpacity(),
            path: [
                [0, 0],
                [2000, 0],
                [3000, 1-winner]
            ]
        });
        this.clickTimeline.set(3000, {duration: 3000});
    }

Note how clickTimeline's `.set()` call at the bottom will run the animations whenever `_spinCard()` is triggered.

<div class="sidenote--other">
<p><strong>Modified file:</strong> <a href="https://github.com/Famous/lesson-visablack-steps/blob/step7/AddCardSwipe/src/start/VisaAd.js">VisaAd.js</a></p>
</div>


<div class="sidenote">
<p><strong>Section recap:</strong> <a href="https://github.com/Famous/lesson-visablack-steps/blob/step7/AddCardSwipe">Branch for this section</a></p>
</div>

<span class="cta">
[Up next: Finish &raquo;](./Finish.html)
</span>