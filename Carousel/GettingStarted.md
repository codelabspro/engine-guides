---
layout: default
title: Getting started
---

<span class="intro-graf">
Before we start coding, let's look at how to download and install Famous so you can develop on your local workstation.
</span>

_If you'd rather dive in first and learn about setup later, skip ahead to the [Hello Famous section](HelloFamous.html)._


## Carousel Seed Project

For simplicity, we've provided a Carousel Seed Project that you can fork with the Famous CLI and use to follow along with the lesson.  First, [install the CLI](http://famous.org/get-started.html).  Then, run:

    $ famous fork carousel-tutorial
    $ cd lesson-carousel-starter-kit
    $ famous dev

The app will be running from a local webserver at [localhost:1618](http://localhost:1618).

## Following along

In the lesson-carousel-starter-kit repo, we’ve created individual branches for each of the steps in the lesson. Simply git checkout the branch whose step you want to look at, and then open the project in your favorite text editor. The branches available are:

  - step1-HelloFamous
  - step2-AddingChildNodes
  - step3-PositioningChildren
  - step4-AddArrowClass
  - step5-AddDotsClass
  - step6-AddPager
  - step7-AddPhysics
  - step8-EmittingHandlingEvents

Before moving on to the next step make sure you are on the branch for the first step by entering:

    $ git checkout master 

Note that the code snippets shown in our examples here might differ somewhat from the code in the repo. We will be working out of the `src/carousel` directory. 



<span class="cta">
[Next up: Hello Famous &raquo;](HelloFamous.html)
</span>
