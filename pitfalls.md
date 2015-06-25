---
layout: default
title: Pitfalls
---

Famous does its best to provide a rich visual experience at 60 FPS. However, some techniques that are commonplace in traditional web development can have a negative impact on Famous’ performance. Here, we’ll cover a few of the main techniques to avoid so that your application will keep humming.

#Pinging the DOM

Famous is very different from many Javascript frameworks, since it endorses zero touches to the DOM. Querying the DOM (selecting elements, querying an element property) can lead to both performance issues as well as unexpected behavior.

In terms of performance, invoking large queries against the DOM is fairly expensive (in measurement of CPU cycles). Also, seemingly simple requests, such as asking for the width of a DOM element, will sometimes cause the entire page to reflow in order to calculate the correct value for the element’s width. These reflows are made more apparent in a highly animated environment.

#Using setInterval, setTimeout, etc.

Using the default Javascript `.setInterval` timer function will cause issues in Famous because of the amount of code the engine runs. In some cases, it is even possible for the default `.setInterval` to miss a cycle. If you need `.setInterval`- or `.setTimeout`-type functionality, use Famous’ clock utility instead:

```javascript
var FamousEngine = require('famous/core/FamousEngine');
var clock = FamousEngine.getClock();
clock.setTimeout(function(){ /*...*/ }, 100);
clock.setInterval(function(){ /*...*/ }, 100);
```

#Using native DOM events

Using native DOM events is fine for intra-node eventing but is messy for inter-node eventing. When trying to communicate across nodes, it is best to use Famous’ own eventing system. By keeping all events in a single system, your application will be much easier to manage.

#Non-performant CSS

CSS is from the age of web pages, not web applications. As a result, the long paint and reflow times associated with certain CSS classes has gone somewhat unnoticed due to the static nature of the web. Below are some of the CSS pitfalls a developer may run into in a highly dynamic environment:

Combining border-radius and box-shadow: Try to avoid this technique as it will lead to performance problems, especially during animations. See the CSS Paint Times and Page Render Weight for a great explanation.

Breaking the bounding box. Using CSS to position child elements outside of the bounding box of their parent will lead to performance issues. While it is common to see DOM elements absolutely positioned, e.g. left: -5px, in the wild, this technique actually makes it harder for browsers to paint the element performantly.

Transitioning color values: This causes large amounts of repainting and is very non-performant and should be used sparingly. In some cases, a similar effect may be achieved by stacking a handful of elements of different colors and opacitating them in/out accordingly.

Check out <a href="http://www.html5rocks.com/en/tutorials/speed/high-performance-animations/">High Performance Animations</a> for a great rundown of CSS’ impact on performance.