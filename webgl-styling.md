---
layout: default
title: WebGL Styling
---

Once a Mesh's Geometry is set, you can style it with colors, Materials, or various lighting effects. 

## Adding color

The most basic way to style a mesh is to give it a color. Here, we use the `.setBaseColor()` method with the Color utility. 


    var Color = require('famous/utilities/Color')
    
    var mesh = new Mesh(node)
    mesh.setGeometry('Box')
    
    mesh.setBaseColor(new Color('blue'))

The Color utility accepts a color name, Hex color string, RGB color array ([r,g,b]), or another Color instance as its first parameter.

The `.setBaseColor()` method is one of four inputs you can use to 'style' a mesh. Since `.setBaseColor()` feeds into a lighting equation, you can also use `.setBaseColor()` to add a Material. Below, we will introduce Materials and then go over the other three inputs that you can use in place of or in addition to `.setBaseColor()`.

## What are Materials?

Think of a Material as the paint that covers a 3D object. A Material is constructed by adding images or expressions to a Material instance. Similar to an artist that uses light and dark shades to give a painting depth, GL uses a _shader_ to take the Material data and calculate the final color result at different points of the object. 

## Adding Materials

The first and most important thing to know about Materials is that they are
constructed not through code, but via a network of scripting nodes. Each node
contains a snippet of GLSL, designated to perform a specific task. This means that as
you construct a Material, you are creating GLSL code through scripting.

Each node is a function that takes in n number of inputs and produces a single
output. 

    var image = Material.Texture('/images/lol.png');
    var time = Material.time();
    var mix = Material.mix([image, [1,1,0,1], time]);

    function customMaterialView(node) {
      var mesh = new Mesh(node);

      mesh.setGeometry('sphere');
      mesh.setBaseColor(mix);
    }

The code above is a simple shader to read in a texture and filter out the green
channel as a function of time. Note that a Texture is simply an image that get's added to a material before shading occurs.  

## Shading a Mesh

A Mesh defines four inputs that are fed into the lighting equation. Each of these inputs accepts a Material or a float as a parameter to customize the appearance of the mesh.


  - `.setBaseColor()` defines the overall color of the Mesh. It takes in a Vector3 (RGB)
  value and each channel is automatically clamped between 0 and 1.

  - `.setGlossiness()` controls how rough the Mesh is. A rough Mesh will scatter reflected
  light in more directions than a glossy Mesh. This can be seen in how blurry or
  sharp the reflection is or in how broad or tight the specular highlight is.

  - `.setNormals()` takes in a (vec3) normal map, which is used to provide significant
  physical detail to the surface by perturbing the "normal," or facing direction, of
  each individual pixel.

  - `.setPositionOffset()` takes in a (vec3) normal map, which is used to provide significant
  physical detail to the surface by perturbing the "normal," or facing direction, of
  each individual pixel.

## Expression Reference

Below are a list of expressions you can use to contruct your materials.

`.abs()`: The abs function returns the absolute value of x, i.e. x when x is positive or
zero and -x for negative x. The input parameter can be a floating scalar or a float
vector. In case of a float vector the operation is done component-wise.

`.sign()`: The sign function returns 1.0 when x is positive, 0.0 when x is zero and -1.0
when x is negative. The input parameter can be a floating scalar or a float
vector. In case of a float vector the operation is done component-wise.

`.floor()`: The floor function returns the largest integer number that is smaller or
equal to x. The input parameter can be a floating scalar or a float vector. In case
of a float vector the operation is done component-wise.

`.ceiling()`: The ceiling function returns the smallest number that is larger or equal to
x. The input parameter can be a floating scalar or a float vector. In case of a float
vector the operation is done component-wise.

`.min()`: The min function returns the smaller of the two arguments. The input parameters
can be floating scalars or float vectors. In case of float vectors the operation is
done component-wise.

`.max()`: The max function returns the larger of the two arguments. The input parameters
can be floating scalars or float vectors. In case of float vectors the operation is
done component-wise.

`.clamp()`: The clamp function returns x if it is larger than minVal and smaller than
maxVal. In case x is smaller than minVal, minVal is returned. If x is larger than
maxVal, maxVal is returned. The input parameters can be floating scalars or float
vectors. In case of float vectors the operation is done component-wise.


`.mix()`: The mix function returns the linear blend of x and y, i.e. the product of x and
(1 - a) plus the product of y and a. The input parameters can be floating scalars or
float vectors. In case of float vectors the operation is done component-wise.

`.step()`: The step function returns 0.0 if x is smaller then edge and otherwise 1.0. The
input parameters can be floating scalars or float vectors. In case of float vectors
the operation is done component-wise.

`.smoothstep()`: The smoothstep function returns 0.0 if x is smaller then edge0 and 1.0
if x is larger than edge1. Otherwise the return value is interpolated between 0.0 and
1.0 using Hermite polynomirals. The input parameters can be floating scalars or float
vectors. In case of float vectors the operation is done component-wise.

`.sin()`: The sin function returns the sine of an angle in radians. The input parameter
can be a floating scalar or a float vector. In case of a float vector the sine is
calculated separately for every component.

`.cos()`: The cos function returns the cosine of an angle in radians. The input parameter
can be a floating scalar or a float vector.

`.pow()`: The power function returns x raised to the power of y. The input parameters can
be floating scalars or float vectors. In case of float vectors the operation is done
component-wise.

`.sqrt()`: The sqrt function returns the square root of x. The input parameter can be a
 floating scalar or a float vector. In case of a float vector the operation is done
 component-wise.

## Adding light 

By default, all famous scenes are lit by a white ambient light. Adding a PointLight
to the scene will turn off this default light. This behavior can be overridden by
adding an AmbientLight to the context. Lights use the color component for easily changing colors.

Like meshes and DOMElement components, Lights take their position from the node upon which they are attached. This allows you to easily position your lights hierarchically in the scene. It also allows you to easily debug and 'see' the light source position by adding a
DOMElement component to the same node as the light.

    
    var PointLight = require('famous/webgl-renderables/PointLight')
    var AmbientLight = require('famous/webgl-renderables/AmbientLight')
    var Color = require('famous/utilities/Color')

    function addLight(node) {
       this.light = new PointLight(node);
       this.ambientLight = new AmbientLight(node)

       node.setPosition(300, 400, 500);
       this.light.setColor(new Color('red'))

       //turns the default ambient light back on for the node
       this.ambientLight.setColor(new Color('white'))
    }
    


Currently, Famous supports 4 point lights simultaneously. Point Lights work much like a real world light bulb, emitting light in all directions from the light bulb's tungsten filament. For the sake of simplicity, Point Lights are approximated to emitting light equally in all directions from just a single point in space.
