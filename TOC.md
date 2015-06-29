## Table of Contents

### Overview
['Hello Famous'](hello-famous.md)
['Download & install'](download-and-install.md)
['Browser & device support'](browser-and-device-support.md)
[Contributing](contributing.md)
### Essentials
[Boilerplate](boilerplate.md)
[Scene graph](scene-graph.md)
[Components](components.md)
[HTML Content](displaying-content.md)
[HTML Styling](styling-content.md)
[Positioning](positioning.md)
[Sizing](sizing.md)
[User input](user-input.md)
[Gesture Handler](gesture-handler.md)
### Intermediate
[Transitionables](transitionables.md)
[Easing curves](easing-curves.md)
[Program events](program-events.md)
[Physics](physics.md)
['WebGL Content'](webgl-content.md)
['WebGL Styling'](webgl-styling.md)
###Help
[Pitfalls](pitfalls.md)
[FAQ](faq.md)
[Glossary](glossary.md)
[Resources](resources.md)

<!-- _config.yml file 
title: Guides | Famous Platform
navigation:
  -
    name: Overview
    children:
      -
        name: 'Hello Famous'
        path: hello-famous
        next: 'Download & install'
        nextPath: download-and-install
        children:
      -
        name: 'Download & install'
        path: download-and-install
        previous: 'Hello Famous'
        previousPath: hello-famous
        next: 'Browser & device support'
        nextPath: browser-and-device-support
        children:
      -
        name: 'Browser & device support'
        path: browser-and-device-support
        previous: 'Download & install'
        previousPath: download-and-install
        next: Contributing
        nextPath: contributing
        children:
      -
        name: Contributing
        path: contributing
        previous: 'Browser & device support'
        previousPath: browser-and-device-support
        next: Boilerplate
        nextPath: boilerplate
        children:
  -
    name: Essentials
    children:
      -
        name: Boilerplate
        path: boilerplate
        previous: Contributing
        previousPath: contributing
        next: Scene graph
        nextPath: scene-graph
        children:
      -
        name: Scene graph
        path: scene-graph
        previous: Boilerplate
        previousPath: boilerplate
        next: Components
        nextPath: components
        children:
      -
        name: Components
        path: components
        previous: Boilerplate
        previousPath: boilerplate
        next: Displaying content
        nextPath: 'displaying-content'
        children:
      -
        name: HTML Content
        path: 'displaying-content'
        previous: Components
        previousPath: components
        next: Styling content
        nextPath: 'styling-content'
        children:
      -
        name: HTML Styling
        path: 'styling-content'
        previous: Displaying content
        previousPath: 'displaying-content'
        next: Positioning
        nextPath: 'positioning'
        children:
      -
        name: Positioning
        path: 'positioning'
        previous: Styling content
        previousPath: 'styling-content'
        next: Sizing
        nextPath: 'sizing'
        children:
      -
        name: Sizing
        path: 'sizing'
        previous: Positioning
        previousPath: 'positioning'
        next: User input
        nextPath: 'user-input'
        children:
      -
        name: User input
        path: 'user-input'
        previous: Sizing
        previousPath: 'sizing'
        next: Gesture Handler
        nextPath: 'gesture-handler'
        children:
      -
        name: Gesture Handler
        path: 'gesture-handler'
        previous: User input
        previousPath: 'user-input'
        next: 'Animation: Transitionables'
        nextPath: 'transitionables'
        children:
  -
    name: Intermediate
    children:
      -
        name: Animation
        children:
          -
            name: Transitionables
            path: 'transitionables'
          -
            name: Easing curves
            path: 'easing-curves'
      -
        name: Program events
        path: 'program-events'
        previous: 'Animation: Easing curves'
        previousPath: 'easing-curves'
        next: Physics
        nextPath: 'physics'
        children: ~
      -
        name: Physics
        path: 'physics'
        previous: Program Events
        previousPath: 'program-events'
        next: 'WebGL Content'
        nextPath: 'webgl-content'
        children: ~
      -
        name: 'WebGL Content'
        path: 'webgl-content'
        previous: Program Events
        previousPath: 'program-events'
        next: 'WebGL Styling'
        nextPath: 'webgl-styling'
      -
        name: 'WebGL Styling'
        path: 'webgl-styling'
        previous: WebGL Content
        previousPath: 'webgl-content'
  -
    lessons: Lessons
  -
    name: Help
    children:
      -
        name: Pitfalls
        path: 'pitfalls'
        next: FAQ
        nextPath: 'faq'
        children: ~
      -
        name: FAQ
        path: 'faq'
        previous: Pitfalls
        previousPath: 'pitfalls'
        next: Glossary
        nextPath: glossary
        children: ~
  -
    name: Glossary
    path: glossary
    previous: FAQ
    previousPath:
    next: Resources
    nextPath: resources
    children: ~
  -
    name: Resources
    path: resources
    previous: Glossary
    previousPath: glossary
    children: ~

lessons:
  -
    title: Carousel
    navigation:
      -
        name: Welcome
        path: index
      -
        name: Getting Started
        path: GettingStarted
      -
        name: Hello Famous
        path: HelloFamous
      -
        name: Architecture
        path: Architecture
      -
        name: Layout
        path: Layout
      -
        name: Organizing Code
        path: OrganizingCode
      -
        name: Arrows
        path: Arrows
      -
        name: Dots
        path: Dots
      -
        name: Pager
        path: Pager
      -
        name: Animating with Physics
        path: AnimatingWithPhysics
      -
        name: User Interaction
        path: UserInteraction
      -
        name: Finish
        path: Finish
  -
    title: Twitterus
    navigation:
      -
        name: Welcome
        path: index
      -
        name: Getting Started
        path: GettingStarted
      -
        name: Creating an App
        path: CreatingAnApp
      -
        name: Architecture
        path: Architecture
      -
        name: Layout
        path: Layout
      -
        name: Header
        path: Header
      -
        name: Footer
        path: Footer
      -
        name: Swapper
        path: Swapper
      -
        name: User Interaction
        path: UserInteraction
      -
        name: Animation
        path: Animation
      -
        name: Finish
        path: Finish
  -
    title: Mixed Mode
    navigation:
      -
        name: Intro
        path: index
      -
        name: Using Meshes
        path: UsingMeshes
      -
        name: 'Lights, Camera, Action'
        path: LightsCameraAction
      -
        name: Mixing Modes
        path: MixingModes
      -
        name: Organizing Our Scene
        path: OrganizingOurScene
      -
        name: Importing The Model
        path: ImportingTheModel
      -
        name: The Iframe Screen
        path: TheIframeScreen
      -
        name: Details Details
        path: DetailsDetails
      -
        name: Finish
        path: Finish
  # -
  #   title: Visa Black
  #   navigation:
  #     -
  #       name: Welcome
  #       path: index
  #     -
  #       name: Getting Started
  #       path: GettingStarted
  #     -
  #       name: What's Included
  #       path: WhatsIncluded
  #     -
  #       name: Architecture
  #       path: Architecture
  #     -
  #       name: The Banner
  #       path: Banner
  #     -
  #       name: Text
  #       path: Text
  #     -
  #       name: Card
  #       path: Card
  #     -
  #       name: Info Section
  #       path: InfoSection
  #     -
  #       name: Timeline
  #       path: Timeline
  #     -
  #       name: User Interaction
  #       path: UserInteraction
  #     -
  #       name: Finish
  #       path: Finish
  # -
  #   title: 'Embedding Projects'
  #   navigation:
     # -
     #   name: Welcome
     #   path: index
     # -
     #   name: Getting Started
     #   path: GettingStarted
     # -
     #   name: Adding Containers
     #   path: AddingContainers
     # -
     #   name: Embedding Containers
     #   path: EmbeddingContainers
     # -
     #   name: Finish
     #   path: Finish
-->