---
layout: default
title: What's included
---

<span class="intro-graf">
The Famous platform comes bundled with a variety of modules and utility classes that provide all you need to build an interface. You don't need to know about all of them to use Famous, but you may want to keep this page handy as a reference later.
</span>

## Components

Rendering components that can be attached to render nodes.

* Align
* Camera
* EventEmitter
* EventHandler
* GestureHandler
* UIEventHandler
* MountPoint
* Opacity
* Origin
* Position
* Rotation
* Scale
* Size

## Core

Core componentry of the platform. Mainly used internally.

* Align
* ComponentStore
* GlobalDispatch
* Context
* Famous
* Layer
* Clock
* LocalDispatch
* MountPoint
* Node
* Opacity
* Origin
* RenderContext
* RenderProxy
* Size
* Transform

## DOM Renderables

Components that can be rendered in the DOM.

* DOMElement

## DOM Renderers

Modules for managing components that can be rendered in the DOM.

* DOM Renderer
* Element Cache

## Engine

Manages the application's `requestAnimationFrame` loop.

<!-- ## Handlers

Modules that register and trigger UI and program events.

* EventHandler
* UIEvents
* Swipe
* RenderHandler -->

## Math

Suite of math utilities. Primarily used internally.

* Mat33
* Quaternion
* Vec2
* Vec3

## Physics

3D physics engine and related helper classes and modules.

* Angle
* Box
* Collision
* Constraint
* ConvexBodyFactory
* Curve
* Direction
* Distance
* Drag
* Force
* Geometry
* Gravity1D
* Gravity3D
* Hinge
* Particle
* PhysicsEngine
* Point2Point
* RotationalDrag
* RotationalSpring
* Sphere
* Spring
* Wall

## Polyfills

Required polyfills for browser rendering.

## Renderers

Rendering machinery used internally by the platform.

* Compositor
* ThreadManager
* VirtualObservable
* VirtualWindow

## Router

Simple module for routing in single-page applications.

* History
* Router

## Stylesheets

Required CSS for correct DOM rendering.

## Transitions

Modules for creating and managing smooth animations.

* Curves
* Transitionable

## Utilities

General-purpose utility classes and helpers.

* CallbackStore
* clone
* flatClone
* KeyCodes
* loadURL
* MethodStore
* ObjectManager
* Color
* ColorPalette
* strip

## WebGL Geometries

Collection of built-in WebGL geometries and generators.

* Box
* Circle
* Cylinder
* GeodesicSphere
* Icosahedron
* ParametricCone
* Plane
* Sphere
* Tetrahedron
* Torus
* Triangle
* GeometryHelper
* DynamicGeometry
* Geometry
* OBJLoader

## WebGL Materials

Colection of WebGL materials and material helpers.

* Material
* TextureRegistry

## WebGL Renderables

Components that can be rendered in WebGL.

* Mesh
* PointLight
* AmbientLight
* Light

## WebGL Renderers

Modules for managing components that can be rendered in WebGL.

* Buffer
* BufferRegistry
* Checkerboard
* Program
* WebGLRenderer
* Texture

## WebGL Shaders

Vertex shaders, fragment shaders, and other GLSL chunks.

* applyLight
* applyMaterial
* convertToClipSpace
* FragmentShader
* getNormalMatrix
* inverse
* invertYAxis
* lowpRandom
* random
* transpose
* VertexShader
