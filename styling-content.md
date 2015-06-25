---
layout: default
title: Styling content
---

<span class="intro-graf">
 While we recommend using external CSS files for styling content, Famous gives us several options for styling [DOM Element components](displaying-content.html) both inline and externally. 
</span>


## Basic Styling

Use the DOM Element component's `.setProperty()` method to add a single inline CSS style.

    var basicElement = new DOMElement(rootNode);
    // add inline styles
    basicElement.setProperty('background-color', 'teal')
                .setProperty('border-radius', '50%')
                .setProperty('border', '2px solid black')

The `.setProperty()` method takes a stringified CSS property name and value as its only two parameters. _Note how the methods can be chained_. 

## Adding Multiple Styles

When creating a new DOM element component, you can also add multiple CSS styles through an _options object_ passed to its constructor. In the options object, use a `properties` key associated with an object containing the CSS property names and values stored as key value pairs.

    // does the same as the code above
    var childEl = new DOMElement(rootNode, {
      properties: {
        'background-color': 'teal',
        'border-radius': '50%',
        'border': '2px solid black'
      }
    })

_Note that hyphenated CSS property names must be stringified._


## Best Practice: Adding Classes and IDs

In line with HTML conventions, it's a better practice to control the style of elements through external CSS files. To do this, Famous let's you add classes and IDs as you would in a traditional HTML document.

Classes and ids of any element can be set through the DOM Element constructor's _options object_. Provide the options object an `id` as a string and `classes` as an array of class names.

     var element = new DOMElement(node, {
      id: 'my-custom-id',
      classes: ['class-one', 'class-two']
     });

Additionally, classes and IDs can be altered at any point using the `.setId()`, `.addClass()`, and `.removeClass()` methods.

    element.addClass('class-three');
    element.removeClass('class-two');
    element.setId('ice-cream');


It is important to note that you can also include classes and IDs in the HTML strings passed to the `.setContent()` method.

    element.setContent('<h1 id="header-main"> This is my main header </h1><h2 class="sub-header"> This is my sub-header </h2>');


