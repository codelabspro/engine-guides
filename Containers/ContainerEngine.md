---
layout: default
title: Using the Container Engine
---

When deploying your application as a container, each application would usually
start its own `requestAnimationFrame` loop. In order to prevent this form
happening and pass down window events, a different Engine needs to be used
when deploying to Hub.

Instead of setting up your application using the usual boilerplate:


    var Engine = require('famous/engine/Engine');

    var Compositor = require('famous/renderers/Compositor');
    var FamousEngine = require('famous/core/FamousEngine');

    var threadManager = new ThreadManager(
        FamousEngine.getChannel(),
        new Compositor(),
        new Engine()
    );

require and use the customized ContainerEngine:


    var ContainerEngine = require('famous/engine/ContainerEngine');

    var Compositor = require('famous/renderers/Compositor');
    var FamousEngine = require('famous/core/FamousEngine');

    var threadManager = new ThreadManager(
        FamousEngine.getChannel(),
        new Compositor(),
        new ContainerEngine()
    );

The ContainerEngine is fully API compatible with the regular Engine. The only
difference is that the requestAnimationFrame loop runs in the actual window
context as opposed to the iframe itself.

Each container has its own ContainerEngine. Starting and stopping the
ContainerEngine does not affect the requestAnimationFrame loop, but pretends to
have the same effect. That way your containers are running in completely
self-encapsulated black-boxes, without being able to influence each other
without the container manager.

## Container Messages

The communication between the renderer running in the UI Thread and the
application code running in a Worker/ self-encapsulated environment is by design
already based on message passing.

In order to synchronize the time across containers, the Container Manager is
running the requestAnimationFrame and sends down the respective `FRAME` commands
to the container. The ContainerEngine then intercepts those messages and routes
them to the ThreadManager.

The ThreadManager then has the ability to act accordingly.
