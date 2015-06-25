## Welcome to the Famous Engine Prose Guides Repo! 

This repo contains all of the raw Markdown content for the [famous.org/learn](https://famous.org/learn) section of the website. Note that Tutorials are broken into Folders, while the guides are located in the root directory of the repo. 

We've exposed this content so that you can help us provide more accurate and comprehensive learning resources. While this repo does not expose any of the website's markup, feel free to file issues here about broken links or inconsistencies with the famous.org website.

## Style tips 

(borrowed from MDN & jQuery documentation)

_The following norms should be followed for new code and existing code that needs cleanup. Note that these are only guidelines so there always may always be exceptions to the rules._



## Comments accompanying code

Use two slashes for comments: `//`. Comments should be formated in sentence case with a single space between the start of the comment and the text. Comments should not end with punctuation unless followed by another sentence:
    
    // This is a comment that will be broken into
    // two separate sentences. Note the single  
    // period in between

## Method names

Always include a period and parenthesis when referencing a method name i.e. `.methodName()`. Use back ticks when referencing any method name, variable, or property within text. 


## More style tips

 - No whitespace at the end of line or on blank lines.
 - Lines should be no longer than 80 characters, and must not exceed 100 (counting tabs as 4 spaces). There are 2 exceptions, both allowing the line to exceed 100 characters:
 - If the line contains a comment with a long URL.
 - If the line contains a regex literal. This prevents having to use the regex constructor which requires otherwise unnecessary string escaping.
 - if/else/for/while/try always have braces and always go on multiple lines.
 - Unary special-character operators (e.g., !, ++) must not have space next to their operand.
 - Any , and ; must not have preceding space.
 - Any ; used as a statement terminator must be at the end of the line.
 - Any : after a property name in an object definition must not have preceding space.
 - The ? and : in a ternary conditional must have space on both sides.
 - No filler spaces in empty constructs (e.g., {}, [], fn())
 - New line at the end of each file.
 - If the entire file is wrapped in a closure, the function body is not indented.
 - Two spaces should be used for code examples. 

## Format

All prose should be saved as Markdown (.md). Files should begin with the following heading where the file's `title` is listed in title case. 

    ---
    layout: default
    title: The Tile
    ---

Note that there is only one title per page and this should be used in place of a pound ( `#` ) to delimit a title. 

In your md files, use two pound signs with a single space for subheadings. Only the first letter is caplitalized (sentence case).

     ## Your heading



## Code 

When including inline code (within text) we use  two backticks around the &#96;code&#96;.

For block sections of code or any code example, use four spaces.
    
    // This is four spaces out


## Guidelines for building tutorials:

While we try to limit the pronouns `'we'` and `'you'` in the prose guides, the tutorials can be more informal. However, all of the guidelines above still apply for tutorials as well as the additional markup and formatting guidelines listed below.

## Intro sections  

Use the `intro-graf` class for the first main _intro_ section to your page like so: `<span class="intro-graf"></span>`. 

In these sections, try to answer: What you're doing and Why you're doing it?

## Images

Add images to [./assets/images](#) folder and include them using this format: 

`![image desc](./assets/images/yourimagename.png)`

## Links to repo 

For modified files/links back to the repo we use the `sidenote--other` class with the following format:
    
    <div class="sidenote--other">
    <p><strong>Modified files:</strong> <a href="##">ModifiedFile.js</a></p>
    </div>

## Section repos

Section recaps (link to the repo at that step) use the `sidenote` class with this format:

    <div class="sidenote">
    <p><strong>Section recap:</strong> <a href="##">Code for this step</a></p>
    </div>


## Up next buttons

We use the following format:

    <span class="cta">
    [Up next: Your next section name &raquo;](./Yournextsectionname.html) 
    </span>

