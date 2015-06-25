---
layout: default
title: Displaying content
---

<span class="intro-graf">We use _DOM element components_ to display traditional HTML elements within Famous applications.
 Every DOM element component attaches to an application through a single scene graph node. </span>

## Hello World!

To add a DOM Element component, pass a node to the DOMElement constructor as its first parameter. The constructor also accepts a second _options hash_ parameter.
    
    var DOMElement = require('famous/dom-renderables/DOMElement')
    // add an element
    var basicElement = new DOMElement(rootNode, {
        content: 'Goodbye World!'
    });
    // set its content
    basicElement.setContent('Hello World!')

The `.setContent()` method sets the given string argument as the content of the displayed element. 

## HTML content

We aren't limited to plain text content; DOM element components also accept HTML content. Pass an HTML string to the `.setContent()` method just like we would a plain text string.

    basicElement.setContent('<em>Here</em> is some <strong>formatted</strong> text content.')

A basic DOM element component is equivalent to a `<div>`, so it can accept any valid HTML that could go into a `<div>` in a normal HTML document.

## Images & other elements

DOM element components can represent elements other than `<div>`s, such as `<img>` or `<video>` or even `<iframe>`. 

For changing the tag, pass the new tag name in an _options object_ to the DOM element constructor. As its second parameter, the constructor accepts an object with a `tagName` key and the new HTML tag name as its value.
    
    var titleElement = new DOMElement( node, { tagName: 'h1' } ); // creates an <h1> element
    var inputElement = new DOMElement( otherNode, { tagName: 'input' } ); // creates an <input>


## Element attributes

Any attribute that is valid HTML5 can be set on DOM Element components. Add attributes by calling the `.setAttributes()` method on a DOM element component or by passing them to the constructor in an options object.

The `.setAttribute()` method takes an attribute name as the first argument, and the value as the second.
    
    titleElement.setAttribute('lang', 'ar') // Set language to Arabic
                .setAttribute('dir', 'rtl') // Set right-to-left text direction
                .setAttribute('contenteditable', true) // Etc.
                .setAttribute('allowfullscreen', true)
                .setContent('السلام عليكم');

When passing attributes to the constructor in an options object, add an `attributes` property with the attribute names and values stored in an object as key value pairs.
   
    // This does the same as the code above
    var titleElement = new DOMElement(child, { 
        attributes: {
          lang: 'ar',            
          dir: 'ltr'             
          contenteditable: true, 
        }
    });

    titleElement.setContent('السلام عليكم');


