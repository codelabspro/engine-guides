---
layout: default
title: Getting started
---

<span class="intro-graf">
Before we start coding, let's look at how to download and install Famous so you can develop on your local workstation. 
</span>

_If you’d rather dive in first and learn about setup later, skip ahead to the [What's Included section](./WhatsIncluded.html)._

##Visa Black lesson starter kit

For simplicity, we've provided a [Visa Black lesson starter kit](https://github.com/Famous/lesson-visablack-steps/tree/master)
that you can clone down and use to follow along with the lesson. Assuming you already have Git installed, your first step is to clone the repo:

    $ git clone https://github.com/Famous/lesson-visablack-steps



From the lesson-visablack-steps project you just cloned down, install the dependencies and then start up the local development server like so:

    $ npm install
    $ npm run start-dev

The app will be running from a local webserver at [localhost:1337](http://localhost:1337/). 

In the `lesson-visablack-steps` repo, we’ve created individual branches for each step in the lesson. Simply `git checkout` the branch whose step you want to look at, and then open the project in your favorite text editor. The branches available are:

  - `step1/AddTimeline`
  - `step2/AddText`
  - `step3/AddCard`
  - `step4/AddInfoSection`
  - `step5/AddFirstFiveSeconds`
  - `step6/AddAllAnimations`
  - `step7/AddCardSwipe`

Note that the code snippets shown in our examples here might differ somewhat from the code in the repo. You will be working from the [start](https://github.com/Famous/lesson-visablack-steps/tree/master/src/start) folder.

Included in the [src](https://github.com/Famous/lesson-visablack-steps/tree/step1/AddTimeline/src) folder of the starter kit is a file called [switch.js](https://github.com/Famous/lesson-visablack-steps/blob/step1/AddTimeline/src/switch.js). By commenting out the top or bottom line you can easily _switch_ between `final` and `start` versions of the project. As you may have guessed, `final` will let you see the finished project in action. 

<span class="cta">
[Up next: What's Included &raquo;](./WhatsIncluded.html)
</span>
