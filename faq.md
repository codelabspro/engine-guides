---
layout: default
title: FAQ
---

## When should I use a Position (or Align, MountPoint, etc) component, when should I set the position of a node directly?

The short answer is that the position component should be used if you would like animate position, but if position is static then you can just set the value of the node. The long answer is that components allow behavior, such as animation, to be applied to nodes. Famous supplies a position component because animating position is such a common task it made sense to provide the component by default. If you are writing a custom component, you should interact with the node directly.

## When should I write components? When should I extend node?

Extending node is a great way to make place a custom controller class in the scene graph. This class can manage and move subnodes, intercept and emit events, and store and manage application state. This is a great way to make a monolithic element of an app and is a sufficient approach for many small scale applications.

However, often with larger applications it gets to the point where behavior starts to diverge from application structure. Regardless of the function that a class serves within the app, there are certain behaviors that it simply must do and these behaviors may be common to several classes in the application. This is where components become the star of the show. Components don't care about where they are in the scene graph, what state exists above or below them, what type of node they are added to, etc. They simply represent reusable scripts. More than that, they are also the easiest way to have some logic happen every frame.

Because components are a safer bet in the long run, it is definitely appropriate to prefer writing components where ever possible for most cases. The best advice is to consider what you are writing. If your class represents an element of the application, such as a header bar, or the application itself, it is probably better to extend node. However, if what you are really interested in is a bit of custom logic, you should write a component. 

## Why is my app crushed above the page?

If you mounted your app onto the body of the document, then it’s likely that you didn’t set the width and height of the html and body elements to be 100%. Because we don’t know where a user’s app will be instantiated we didn’t want to alter the body or html elements in our default CSS. This means that if you want to load a scene on the body you will need to ensure the body is of the proper size in your own CSS.

## Why are elements flickering or invisible?

It’s likely that you are getting what is called z-fighting. In every three dimensional scene, the position along the z axis is used as information to project the elements in the scene onto the two dimensional surface of the screen in some order. If elements in the scene are at the same or, under some projection models, even close to each other in z space, the projection will result in it being difficult to determine which element is in front of the other, causing flickering or unexpectedly blocking an element.
A common source of z-fighting is if a DOM element is at the the exact same z depth as its parent. The element may find itself behind its parent, which obscures it. This is something that is always worth remembering: because this is a 3d scene, DOM hierarchy does not guarantee visibility, which is to say, parents can be in front of their children.

## Can you run the latest version of famous without node.js like you could with the older version?

We have not yet put together documentation, but you can find a global object build on our cdn by following the same url scheme as before

—> [http://code.famo.us/famous/0.5/famous.min.js](http://code.famo.us/famous/0.5/famous.min.js)

Please contact us with any questions. TO NOTE: It will not be possible to do anything with GL textures if you do not use a development server. When developing on a local system you need to use a local server, rather than running files directly off the file system, or you will have CORS errors. In production, it will work absolutely fine as long as the texture and code are on the same domain. GL specifically does not support file system textures for securirty reasons.

## What is the status of Famous-Angular now that the new Famous Engine has been released?

We do not plan on integrating Angular 1.0 with the new Famous Engine. For the current Famous-Angular project, there are several community members who have expressed interest in maintaining it so we may hand it off to them. 

## Where is the old version of  famo.us (http://famo.us/), Famo.us University, and the 0.3.5 documentation?
 
 You can find all of this at http://deprecated.famous.org (http://deprecated.famous.org/)

