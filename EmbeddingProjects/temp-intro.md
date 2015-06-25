---
layout: default
title: How to Deploy a Famous Project
---

<span class="intro-graf">With Famous Cloud Services, upgrading an existing website with stunning UI elements has never been easier. If you know how to embed a basic image or video, then you're ready to start embedding entire Famous projects into websites.
</span>



## Famous Cloud Services Command Line Utility

With the tool you can easily register, manage user accounts, create, update and share projects.

Note that the Famous Hub is in private BETA. Please use the Famous Hub and CLI as a playground knowing that the API may change. For now, follow the steps listed below. 

## Install the CLI

Execute the following command to install the CLI tool. This will make embedding future projects quick and easy.

    $ npm install -g famous

Once installed, executing `$ famous` will display the default help string.

## Download Mix Mode Seed Projet


Within an empty directory, simply run

    $ famous create

This command will download the Mix Mode Seed Project and integrate it within our Famous Cloud Services.

Otherwise, give it a name and it will create the project in a sub folder 

    $ famous create <seed-project-name>

## Develop your project

You can develop your seed project locally using the command below. This will install dependencies, build your project and serve it on port 1337.

    $ famous develop

Note: If you wish to work form a custom project, make sure your package.json contains the same scripts that are listed in the Mixed Mode Seed Project.

### Register with Famous Cloud Services

    $ famous register

Registering allows you to push your seed projects to the cloud.

### Login

    $ famous login

and follow the prompts to login to your Famous Cloud Services user account. 

Note: that if you ran `$ famous register` you are automatically logged in.

### Deploy

This will build your project and save a snap shot into our cloud infrastructure.

    $ famous deploy

Once you are logged in you can deploy your seed project to the cloud. This command will build your project and deploy it to our Famous Cloud Services. Your project will be viewable by visiting the share link or embeddable using the returned HTML.    

The embeddable HTML code should have a `<script>` tag and a `<div>` tag similar to the code below. Both are needed to embed your project.


    <script src="https://demo.famo.us/hub/container/embed.js"></script>
    
    <div class="famous-container" data-famous-container-identifier="your-container-id"></div>


From the terminal, copy both lines of the embeddable HTML code before moving on to the next section. 

Note: The structure of your project should match the same than the Mix Mode Seed project (package.json, /public)


<div class="sidenote--other"><p><b>Tip:</b> Every time you make changes to your project, simply enter <code>$ famous deploy</code> from your the root of your project to update.</p></div>  

Great! Now you can start embedding your own projects and upgrading your sites!

Check out the example below, where we've embedded the project from the [Carousel Lesson] into a Bootstrap Page. Notice how we only need one script tag for embedding multiple projects in a single page. 

[<img style="width:100%" src="./assets/images/bootstrap.png" />](http://learn-staging.famo.us/lessons/container/assets/example/Carousel%20Template%20for%20Bootstrap.html)



<!-- 
In this lesson, we will leverage the Famous Hub Command Line Interface (CLI) to save a project as a widget, create a Famous container and replace an outdated carousel with the carousel we built in the [Famous carousel lesson](http://learn-staging.famo.us/lessons/carousel/).

<span class="cta">
[Up Next: Getting Started &raquo;](./GettingStarted.html)
</span> -->