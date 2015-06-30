---
layout: default
title: Physics
---

<span class="intro-graf">The physics engine provides a means to create custom animations that are composable, extensible, and natural-feeling. When an animation inspired by some physical phenomenon is desired, and when tweening would be intractable, actually simulating the physics will give better results.</span>


At its core, the physics engine is an update loop that incrementally steps the world forward to reflect the progression of time. Entities that may be placed in the world include different types of physical bodies, including point particles and three-dimensional boxes, and different types of forces and constraints, like gravity, drag, and collision.

The physics engine is its own module entirely. To bind a [Famous](http://famous.org/) view to a body in the physics engine, pass the body in as data to the view, and  use the body's position and orientation to set the position and rotation of the view's node.

To get started with physics, require the math and physics APIs.

    var math = require('famous/math');
    var physics = require('famous/physics');


## The Engine

Typical use of the physics engine will begin with something like:

    var world = new physics.PhysicsEngine(options);


Afterwards, all physical bodies, forces, and constraints will be incorporated into the current engine via `world.add(targets)`.

One may optionally specify a `.origin` and/or `.orientation` in the options hash, or later set these via `PhysicsEngine#setOrigin` and `PhysicsEngine#setOrientation`. Then for a body `b`, one may get the position and orientation of `b` relative to an untranslated origin and unrotation orientation via `PhysicsEngine#getTransform(b)`. If neither the origin or orientation are altered, the position and orientation of the body may be used directly.

The engine is progressed by successively calling `PhysicsEngine#update(time)` with an increasing timestamp.

The engine will fire 'prestep' and 'poststep' events before and immediately after each tick of the engine. Callbacks for the events can be registered, deregistered, and manually triggered via `PhysicsEngine#on` , `PhysicsEngine#off`, and `PhysicsEngine#trigger`.

## Bodies

All physical bodies maintain various quantities like position and orientation, angular and translational velocities and momenta, as well as physical properties including mass, inertia tensors and coefficients of restitution and friction. These properties are set according to the options hash passed to the constructor, which is the only parameter the constructors allow.

### Primitives

### `Particle`

    var Particle = physics.Particle;
    var myParticle = new Particle({mass: 10});


The simplest body, and superclass of all other bodies. All forces and constraints take in instances of `Particle` or subclasses as their targets. Instances of `Particle` are considered point-masses, and will not rotate or carry angular momentum, or collide with other `Particle`'s.

Methods provided by `Particle.prototype` include getters and setters for `Particle` properties, as well as methods to apply forces and impulses.

The particle will fire 'collision:start' and 'collision:end' at the beginning and end of contact with another body. Callbacks for the events can be registered, deregistered, and manually triggered via `Particle#on` , `Particle#off`, and `Particle#trigger`.

### `Sphere`

    var Sphere = physics.Sphere;
    var mySphere = new Sphere({mass: 10, radius: 25});


Extends `Particle`, and represents a sphere shape. Unlike `Particle`, has non-zero size defined by the user supplied `.radius`, and has a non-zero inertia tensor.

`Sphere.prototype` provides getters and setters for radius which will update size and the inertia tensor as side effects.

Here is a very basic physics demo linking a Sphere with a Node. Note how the Sphere is responsible for determining the Node's postion. Click the logo to move the Sphere.

<iframe src="https://staging.famous.org/examples/index.html?block=sphere&amp;detail=false&amp;header=false" style="width:100%;height:600px; margin: 15px 0px 25px 0px" scrolling="no" class="code-block" allowtransparency="true"></iframe>



### `Box`

    var Box = physics.Box;
    var myBox = new Box({mass: 10, size: [100,200,300]});


Extends `Particle`, and represents a rectangular box shape. Its span is defined by user supplied `.size`.

`Box` is implemented using `ConvexBodyFactory`.

### Creating Custom Shapes

### `ConvexBodyFactory`

    var Vec3 = math.Vec3;
    var ConvexBodyFactory = physics.ConvexBodyFactory;
    var CustomPyramid = ConvexBodyFactory([
        new Vec3(100,0,100),
        new Vec3(100,0,-100),
        new Vec3(-100,0,100),
        new Vec3(-100,0,-100),
        new Vec3(0,150,0)
    ]);
    var myPyramid = new CustomPyramid({size: [50, 50, 50], mass: 10});


A class factory. Takes in a `ConvexHull` object, or computes one from an array of `Vec3`'s, and returns a class extending `Particle` with inertia tensor and shape reflecting the input geometry. Used to create custom convex physical shapes.

The size and inertia of the object instantiated using the new constructor will reflect the originally defined shape stretched to match the user supplied `.size`.


### Boundaries

### `Wall`

    var Wall = physics.Wall;
    var rightWall = new Wall({direction: Wall.LEFT}); // bodies coming from the left will collide with the wall
    rightWall.setPosition(1000,0,0);


Extends `Particle`, and represents an infinite, oriented plane. Will not move or rotate, though can be positioned via `.setPosition`. The plane may be oriented via the `.direction`  enum, which takes an integer between 0 and 5. The relevant values are attached to the `Wall` constructor, e.g. `Wall.UP`, or `Wall.LEFT`.

Exists primarily to provide a wall collider target for the `Collision` constraint.

## Forces

Every frame of the physics engine, forces will apply an acceleration to each of their targets. Some forces take both a source and targets parameter, other just take targets. All forces take an options hash as their last parameter. Forces typically take a `.strength` option which specifies their strength.

### `Force`

The superclass of all forces. Provides `.setOptions` which resets the force to reflect a new options hash. The `Force` superclass would typically not be used or extended directly, instead instantiate one of its subclasses.

### `Gravity1D`

    var target = new physics.Particle({mass: 1});
    var Gravity1D = physics.Gravity1D;
    var gravity = new Gravity1D([target], {direction: Gravity1D.DOWN, strength: 300});


A one dimensional constant gravitational force. Either takes a `.direction`  enum and `.constant` or an `.acceleration`  `Vec3` option.

### `Gravity3D`

    var source = new physics.Particle({mass: 100});
    var target = new physics.Particle({mass: 1});
    var Gravity3D = physics.Gravity3D;
    var gravity = new Gravity3D(source, [target], {strength: 300});


An inverse-square gravitational force.


<iframe src="https://staging.famous.org/examples/index.html?block=gravity3d&amp;detail=false&amp;header=false" style="width:100%;height:600px; margin: 15px 0px 25px 0px" scrolling="no" class="code-block" allowtransparency="true"></iframe>


### `Spring`

    var source = new physics.Particle({mass: 100});
    var target = new physics.Particle({mass: 1});
    var Spring = physics.Spring;
    var spring = new Spring(source, [target], {period: 1, dampingRatio: 0.5, length: 50});


A spring force. Pulls the targets to within `.length` of the anchor. If you want to anchor the spring to a specific point of the world, pass in `null` as the source and specify an `.anchor`  `Vec3`.

### `RotationalSpring`

    var source = new physics.Particle({mass: 100});
    var target = new physics.Particle({mass: 1});
    var RotationalSpring = physics.RotationalSpring;
    var rotationalSpring = new RotationalSpring(source, [target], {period: 1, dampingRatio: 0.5});


A spring-like torque. Applies torque to enforce an orientation. If you want to pull the targets to a specific orientation, pass in `null` as the source and specify an `.anchor` `Quaternion`.

### `Drag`

    var target = new physics.Particle({mass: 1});
    var Drag = physics.Drag;
    var drag = new Drag([target], {strength: 0.4});


A force opposing the translational movement of each of its targets.

### `RotationalDrag`

    var target = new physics.Particle({mass: 1});
    var RotationalDrag = physics.RotationalDrag;
    var rotationalDrag = new RotationalDrag([target], {strength: 0.4});


A force opposing the rotation of each of its targets.

## Constraints

Rather than being 'fire and forget' like a force in the engine, each constraint is something that must be solved. Each constraint applies impulses to its targets to correct constraint violations, 'solving' it. The constraints have no knowledge of each other, only knowing what needs to be done to correct themselves. Every constraint is solved multiple times, so as to reduce error that might accumulate over the course of the frame.

### `Constraint`

The constraint superclass. Constraints usually either take a targets parameter, or two body parameters, as well as an option hash. The `Constraint` superclass would typically not be used or extended directly, instead instantiate one of its subclasses.

### `Angle`

    var a = new physics.Box();
    var b = new physics.Box();
    var Angle = physics.Angle;
    var angle = new Angle(a, b);


Applies angular impulses to `a` and `b` to preserve the angles between the orientations as they are on instantiation. Does not take an options hash.

### `Distance`

    var a = new physics.Particle();
    var b = new physics.Particle();
    var Distance = physics.Distance;
    var distance = new Distance(a, b, {period: 1, dampingRatio: 0.5});


Applies impulses to preserve the distance between the positions of `a` and `b` as they are on instantiation. Optionally takes a `.length` option if an alternate distance is desired.

### `Direction`

    var a = new physics.Particle();
    var b = new physics.Particle();
    var Direction = physics.Direction;
    var direction = new Direction(a, b, {period: 1, dampingRatio: 0.5});


Applies impulses to preserve the direction of `b` from `a`, i.e. acts to keep `a` and `b` on the line traced from `a` to `b`.

### `BallAndSocket`

    var a = new physics.Box();
    var b = new physics.Box();
    var BallAndSocket = physics.BallAndSocket;
    var hinge = new BallAndSocket(a, b, {anchor: new math.Vec3(100,100,0)});


Applies impulses and angular impulses to preserve the local position of `anchor` relative to `a` and relative to `b`. Imagine `a` and `b` skewered with two rods, one with a ball on one end, and one with a socket, where the two rods are connected at `anchor`.

### `Hinge`

    var a = new physics.Box();
    var b = new physics.Box();
    var Hinge = physics.Hinge;
    var hinge = new Hinge(a, b, {anchor: new math.Vec3(100,100,0), axis: new Vec3(0,0,1)});


Similar to the `BallAndSocket` constraint, but additionally applies angular impulses to constrain `a` and `b`  to only rotate about `axis`.

### `Curve`

    var target = new physics.Particle();
    var Curve = physics.Curve;
    function f(x,y,z) {
        return x*x + y*y + z*z - 100*100;
    }
    function g(x,y,z) {
        return z;
    }
    var curve = new Curve([target], {period: 1, dampingRatio: 0.5, equation1: f, equation2: g});


Takes the surfaces defined by f(x,y,z) = 0 and g(x,y,z) = 0 and acts to keep the targets on their intersection.

### `Collision`

    var a = new physics.Box({size: [50,50,50]});
    var b = new physics.Box({size: [50,50,50]});
    var rightWall = new physics.Wall({direction: physics.Wall.LEFT});
    rightWall.setPosition(1000,0,0);
    var Collision = physics.Collision;
    var collision = new Collision([a, b, rightWall]); // a and b will bounce off of each other and off of rightWall


Updating the `Collision` constraint has multiple phases. First, possible intersections are tallied via the broad-phase, which determines what bodies are likely to be intersecting based on their positions in space. Next, whether the bodies are actually colliding is determined, and if so, the exact points of contact are found. Finally, the points of contact are entered into a cache that holds the current points of contact between those two bodies. The resolution step for `Collision` then solves each contact separately.

