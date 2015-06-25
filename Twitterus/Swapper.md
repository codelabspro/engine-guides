---
layout: default
title: Swapper
---

<span class="intro-graf">The `Swapper` will display the tweet sections of our app. To build it out, we will create three modules --`Swapper`, `Sections`, and `Tweets`-- each of which will contain its own class. Similar to our `Footer`, we will create the parent `Swapper` class first and then work our way down to the child classes. </span>

### Swapper

![swapper](./assets/images/swapper.png)

Like our `Header` and `Footer`, we will create a `Swapper` class and export it as a module.  The `Swapper` will show the current section and keep the other sections off the screen.

Open up `Swapper.js` and add the code below.

    var data = require('./Data');
    var Section = require('./Section');
    var Node = require('famous/core/Node');
    var Align = require('famous/components/Align');
    var DOMElement = require('famous/dom-renderables/DOMElement');

    // The swapper will hold the sections and swap between them
    // on events
    function Swapper () {
        // subclass Node
        Node.call(this);

        // create a new dom element 
        this.el = new DOMElement(this);

        // store the current section
        this.currentSection = null;

        // create the sections
        this.sections = createSections.call(this);
    }

    // subclass Node
    Swapper.prototype = Object.create(Node.prototype);

    module.exports = Swapper;

As you can see above, we add our sections through an external function called `createSections`. Using the lines below, let's build a `createSections` function.

    function createSections () {
        var result = {};

        // iterate over all the sections in our data
        data.sections.forEach(function (section, i) {
            var child = this.addChild();
            result[section.id] = {
                align: new Align(child),
                section: child.addChild(new Section(i))
            }
        }.bind(this));

        return result;
    }

<div class="sidenote--other"><p><strong>Modified files:</strong> <a href="https://github.com/Famous/lesson-twitterus-starter-kit/blob/step-5/src/twitterus/Swapper.js">Swapper.js</a></p></div>

The code above creates a _node_, _Align component_, and `Section` instance for each _section_ in the `data`. Now, let's create the `Section` class so the code above will run.

### Section

Open up `Section.js` and include the code below to create our `Section` module. 
    
    var data = require('./Data');
    var Node = require('famous/core/Node');
    var DOMElement = require('famous/dom-renderables/DOMElement');
    var Tweet = require('./Tweet');

    function Section (i) {
        // subclass Node
        Node.call(this);

        // create and style a new DOMElement
        this.el = new DOMElement(this).setProperty('overflow-y', 'scroll')
                                      .setProperty('overflow-x', 'hidden');

        // create the tweets in the section.
        this.tweets = createTweets.call(this, i);
    }

    Section.prototype = Object.create(Node.prototype);

    function createTweets (id) {
        var result = [];
        var numberOfTweets = data.sections[id].tweetNumber;
        var tweet;

        // create an array of length equal to the number of tweets and then
        // map over it to create an array of tweets
        for (var i = 0 ; i < numberOfTweets ; i++) {
            // this node will be 100px tall and positioned after the previous one
            // in the array
            tweet = this.addChild()
                        .setSizeMode('default', 'absolute')
                        .setAbsoluteSize(null, 100)
                        .setPosition(0, 100 * i)
                        .addChild(new Tweet());

            result.push(tweet);
        }

        return result;
    }

    module.exports = Section;

<div class="sidenote--other"><p><strong>Modified files:</strong> <a href="https://github.com/Famous/lesson-twitterus-starter-kit/blob/step-5/src/twitterus/Section.js">Section.js</a></p></div>

Note how we use the `createTweets` function to build our `Tweets` in the same way we did for our _sections_ in the previous step. Within the function, each tweet is sized and positioned one after another 100px down. Now let's move on to the Tweets class and module.

### Tweets

Add the code below to `Tweets.js` to create our tweets module.  

    var Node = require('famous/core/Node');
    var DOMElement = require('famous/dom-renderables/DOMElement');
    var data = require('./Data');

    // The tweet class that will render a particular tweet
    function Tweet () {
        // subclass Node
        Node.call(this);

        // create a new DOMElement and style it.
        this.el = new DOMElement(this).setProperty('backgroundColor', getRandomColor())
                                      .setProperty('boxSizing', 'border-box')
                                      .setProperty('lineHeight', '100px')
                                      .setProperty('borderBottom', '1px solid black')
                                      .setProperty('font-size', '12px')
                                      .setContent(getRandomMessage());
    }

    // subclass Node
    Tweet.prototype = Object.create(Node.prototype);

    // Pick a random element from an array
    function random (array) {
        return array[(Math.random() * array.length)|0];
    }

    // create Random message
    function getRandomMessage () {
        return '<b>' + random(data.usernames) +
               ':</b>' + random(data.begin) + random(data.middle) + random(data.end) +
               ' ' + random(data.hashtags) + ' ' + random(data.hashtags);
    }

    // Create a random hex color
    function getRandomColor() {
        // trick to create a range.
        return '#' + Array.apply(null, Array(6)).map(function (_, i) {
            return random('0123456789ABCDEF');
        }).join('');
    }

    module.exports = Tweet;

Above, you'll see we call two functions --`getRandomColor()` and `getRandomMessage()`-- to generate and style random tweets. These functions use data that will be added to `Data.json`. Here is `Data.json`:

    // Holds the data for the application
    {

        sections: [{
            id: 'Home',
            tweetNumber: 50
        }, {
            id: 'Discover',
            tweetNumber: 50
        }, {
            id: 'Connect',
            tweetNumber: 50
        }, {
            id: 'Me',
            tweetNumber: 25
        }],

        usernames: ['@LouieBlooRaspberry','@PonchoPunch','@SirIsaacLime','@StrawberryShortKook','@AlexandertheGrape', '@LittleOrphanOrange'],
        begin: ['Walk towards ', 'Jump on ' ,'Sing to ', 'Dance with ', 'Stare down ', 'Pick up ', 'Hold hands with ', 'Walk around ', 'Shake hands with ', 'Talk to ', 'Point at ', 'Read to ', 'High five ', 'Wave to ' ],
        middle: ['a duck ', 'some fish ', 'a zebra ', 'nine honey badgers ', 'an old gorilla ', 'a ham sandwich ', 'a peanut ', 'Nicolas Cage ', 'a sock ', 'a pillow ', '12 fish ','a potato ', 'your neighbor ', 'a snail '],
        end: ['quickly','and don\'t look back','without shoes', 'and clap your hands', 'and pat your belly', 'and do a jig', 'tomorrow', 'while eating ice cream', 'in the dark', 'at the park', 'with a friend', 'down by the bay', 'in the car', 'and yell'],
        hashtags: ['#harrystyles', '#live', '#boredom', '#mylife', '#hiphop', '#texas', '#november', '#scary', '#best',' #snowman', '#shuffle', '#squats', '#selfie' ]

    };



Now that we have all of our classes built, we are ready to bring it all together and add a `Swapper` instance to `Twitterus`.

<div class="sidenote--other"><p><strong>Modified files:</strong> <a href="https://github.com/Famous/lesson-twitterus-starter-kit/blob/step-5/src/twitterus/Tweet.js">Tweet.js</a></p></div>

### Adding the Swapper

In order to reference the `Swapper` class in `Twitterus`, we will need to import the module using the following line.
   
    var Swapper = require('./Swapper')

Once it's imported at top, create a new instance of `Swapper` in the function `makeSwapper()`.

    // make the swapper
    function makeSwapper (node) {
        // the swapper will be 200 pixels smaller than
        // its parent in Y and otherwise the same size.
        // It will be position 100 pixels below its parent
        // such that it clears the header
        node.addChild()
            .setDifferentialSize(null, -200, null)
            .setPosition(0, 100)
            .addChild(new Swapper());
    }

In the next, section we will make our app respond to user interaction through 'clicks'.

<div class="sidenote--other"><p><strong>Modified files:</strong> <a href="https://github.com/Famous/lesson-twitterus-starter-kit/blob/step-5/src/twitterus/Twitterus.js">Twitterus.js</a></p></div>

<div class="sidenote"><p><strong>Section recap:</strong> <a href="https://github.com/Famous/lesson-twitterus-starter-kit/tree/step-5">Code for this step</a></p></div>


<span class="cta">[Up Next: User Interaction &raquo;](./UserInteraction.html)</span>
 
