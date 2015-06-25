---
layout: default
title: Getting started
---

<span class="intro-graf">
Let's download the [Carousel lesson starter kit](https://github.com/Famous/lesson-carousel-starter-kit) and install the Famous Hub Command Line Interface (CLI) so you can follow along from your local workstation. Installing the CLI will make embedding future projects quick and easy. 
</span>


## Downloading the Carousel (You can use any project)

Assuming you already have [Git](http://git-scm.com/book/en/v2/Getting-Started-Installing-Git) and [Node](https://nodejs.org/) installed, excute the following lines in your terminal to get the starter kit:

    $ git clone git@github.famo.us:learn/lesson-carousel-starter-kit.git
    $ cd lesson-carousel-starter-kit 
    $ npm install

<div class="sidenote--other"><p><b>Note:</b> If you already have the Carousel starter kit installed or if you'd like to follow along with your own project, you can skip ahead to <b>Installing the CLI</b> step.</p></div>


Next, _checkout_ the final version of the project and serve it up using the commands:

    $ git checkout final
    $ npm run start-dev

The final version of the app should be running from a local webserver at [localhost:1337](http://localhost:1337). Running it once will prep the project for the container. Use _control + C_ to exit.

## Installing the CLI

Start by opening up a new terminal window and navigating to a folder where you store your binaries. If you are unsure of what to do here, use the following command to switch to your `usr/local/bin` directory:

    $ cd /usr/local/bin


Next, from within the same directory, clone down the repo and install/symlink it with the following shell commands:
    
    $ git clone git@github.famo.us:hub/famous-cli.git
    $ cd famous-cli
    $ npm link

Check if it's correctly installed by entering:

    $ famous -V

This should output the current version number of the CLI to the terminal. 

## Create an account

Enter the following command to create a new Famous hub account:

    $ famous user create

This will prompt you for an email address and password to use with your new account. After filling out the info and agreeing to the Terms of Service, you will then be logged in an ready to start using the Famous CLI.



<span class="cta">
[Next up: Adding Containers &raquo;](./AddingContainers.html)
</span>
