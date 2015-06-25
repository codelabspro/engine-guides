---
layout: default
title: Adding Containers
---

<span class="intro-graf">We use _containers_ to embed Famous projects within web pages. However, we need to save our project as a _widget_ before we can use a container. Let's go over widets and containers and then create some for our Carousel project.</span>

## What is a Widget?

A widget is any project uploaded to the Famous hub. Every widget is associated with a single unique ID and can be displayed in one or many _containers_. We save projects as widgets so they can be served from our hub.

## What is a container?

A container is the element that you embed into web pages to display Famous widgets. A container is also associated with a single unique ID and can only serve one widget at a time. Once embedded in a page, you can swap out the container's widget and track its analytics through the Famous Hub. 

## Creating a widget


From the terminal window with the Carousel project, hit _control + C_ to stop the server if you haven't already. Then navigate to the public directory using the following command:
    
    $ cd public

<div class="sidenote--other"><b>Note:</b> This folder carries the static <code>index.html</code> file that we want to serve up. When creating or updating a widget, you want to run widget commands from the public directory where your HTML file resides.</div>


Next, execute the following widget command from within the public directory:
    
    $ famous widget create 

You will be prompted for a name and if you would like your widget to be public. For this lesson, name it `Carousel` and enter `Y` so it's public. Your widget's unqiue ID should then be output to the terminal. 

## Creating a container

From the same directory, enter the following command to create a widget:

    $ famous container create

This will prompt you for a name, if you would like to associate the current widget with this repo, and if this container is in production. Name it `Embedded Carousel`, and then enter `Y` and `Y` to create your new container and link it to the widget we created in the previous step.

## Pushing to the container

Now that your widget is associated with a container, we'll need to upload our widget's code to the Famous hub so it can run within the container. Use the following command to `push` your code to the hub:

    $ famous widget push 

This should return the sharable link and embeddable HTML code. If you vist the sharable link in the browser, you should see your full project served up by the Famous hub. 

The embeddable HTML code should have a `<script>` tag and a `<div>` tag similar to the code below. Both are needed to embed your project.



    <script src="https://demo.famo.us/hub/container/embed.js"></script>
    <div class="famous-container" data-famous-container-identifier="your-container-id"></div>


From the terminal, copy both lines of the embeddable HTML code before moving on to the next section. 



<div class="sidenote--other"><p><b>Tip:</b> Every time you make changes to your project, simply enter <code>$ famous widget push</code> from your public directory to update your widget and the container that displays it.</p></div>  


<span class="cta">[Up Next: Embedding containers &raquo;](./EmbeddingContainers.html)</span>