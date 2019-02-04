---
layout: post_default
title:  "Drawing with CSS - part 1: box shadows"
date:   2018-02-07 17:21:00 +4:00:00
categories: ["blog", "programming", "css"]
author: "alicja"
published: true
---
I have been using CSS as a medium for drawing for the past few months and I appreciate its
versatility and wide support. Since it's hard to get started without understanding the 
basic concepts and most of the online guides are not focusing on how to draw images with
CSS, I decided to write a few simple guides on creating single div CSS images. In the first
part I'll focus on drawing using box shadows.

<link rel="stylesheet" href="{{ "/css/box-shadows.css" | prepend: site.baseurl_root  }}">

To understand how to use box shadows for drawing, it's best to see them in action and learn
about different layers we can create with them. Let's start with a black square div and put it 
in a container.

```
.container {
  background-color: white;
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
  height: 200px;
  width: 580px;
  padding-top: 20px;
  padding-bottom: 20px;
}

.boxy {
  background-color: black;
  height: 100px;
  width: 100px;
}
```

<div class="drawing-container"><div class="boxy"></div></div>

The `boxy` element is a base layer for the drawing - every other layer we add is going to be
in some way relative to it (or its parent). The simplest way in which we can add new elements 
without creating new divs is by adding a shadow to `boxy`.

Let's take a closer look at the `box-shadow` property:
- `inset` [optional] - by default the shadow is assumed to be behind the element (as if the element 
was dropping it), setting it to `inset` will cause the shadow to be drawn on the inside 
- `<offset-x>` (px) - the horizontal offset of the shadow from the default position behind the element, 
positive values push it to the right, negative to the left of the element
- `<offset-y>` (px) - the vertical offset of the shadow from the default position behind the element,
positive values push it below the element, negative above the element
- `<blur-radius>` (px) - positive values only, the bigger the value the bigger and lighter the shadow
- `<spread-radius>` (px) - positive values will expand the shadow, negative will decrease it by adding 
or substracting the provided value from each of the sides
- `<color>` (keyword/hex/RGB/HSL) - color of the shadow

Let's add a shadow to the black box - a light sea-green, 60x60px, solid square below the black square.

```
.boxy {
  background-color: black;
  height: 100px;
  width: 100px;

  box-shadow: 
    0 120px 0 -20px lightseagreen;
}
```

<div class="drawing-container"><div class="boxy boxy-shadow"></div></div>

We can add as many box shadows as we want, changing the positions, shapes and colors.
It allows for as many squares as we need, as the box shadows will keep the same shape
as the parent element, that we can place anywhere.

```
.boxy {
  background-color: black;
  height: 100px;
  width: 100px;

  box-shadow: 
    120px 120px 0 -30px cyan, 
    0 120px 0 -20px lightseagreen,
    -120px 120px 0 -30px cyan;
}
```

<div class="drawing-container"><div class="boxy boxy-shadows"></div></div>

But we can have more layers that allow for more shapes - after all, not everything is 
a square. To add an additional layer to the div, we can use the `:before` pseudoelement. 
Pseudoelements in their simplest form <span class="pseudo">to add content before<span> 

But we can use a trick to make them work for creating multi-layered single div images.
The `boxy` element is placed inside a container with a position `relative`, which allows 
us to use `absolute` positioning for the pseudo-elements, directly controling their position.

```
.boxy:before {
  content: "";
  background-color: red;
  height: 60px;
  width: 60px;
  position: absolute;
  left: 160px;
  border-radius: 50%;
}
```

<div class="drawing-container"><div class="boxy boxy-shadows boxy-before"></div></div>

The `:before` pseudoelement can have its own shadows, with their own positons, sizes and colors.

```
.boxy:before {
  content: "";
  background-color: red;
  height: 60px;
  width: 60px;
  position: absolute;
  left: 160px;
  border-radius: 50%;

  box-shadow: 
    -80px 0 0 tomato,
    0 80px 0 -10px tomato;
}
```

<div class="drawing-container"><div class="boxy boxy-shadows boxy-before-shadows"></div></div>

Similarly, we can add an `:after` element with `absolute` positioning, with its own shape and a
set of shadows.

```
.boxy:after {
  content: "";
  background-color: green;
  height: 40px;
  width: 60px;
  position: absolute;
  left: 360px;
  
  box-shadow: 
    80px 0 0 yellowgreen,
    0 80px 0 -10px yellowgreen;
}
```

<div class="drawing-container"><div class="boxy boxy-shadows boxy-before-shadows boxy-after"></div></div>

Adding all that together allows us to have:
- three base layers with three shapes (the div, its `:before` and `:after`)
- unlimited number of shadows for each layer

Using only those elements, we can create pretty complex images - let's work through an example, 
recreating the [Recurse Center](https://www.recurse.com/) logo. We're going to start with a simple 
base div.

```
.rc {
  background-color: black;
  height: 380px;
  width: 300px;
}
```

<div class="drawing-container container-rc"><div class="rc"></div></div>

The next base shape we need is a rectangle with different proportions than the base
element, for which we're going to use a `:before` element (or rather, its shadows).

```
.rc:before {
  content: "";
  background-color: red;
  height: 200px;
  width: 250px;
  position: absolute;
  top: 20px;
  left: 320px;
}
```

<div class="drawing-container container-rc"><div class="rc rc-before-show"></div></div>

First we're going to use the `:before` element for the screen - both the white border 
and the black interior of it. We can stack box shadows on top of each other, starting 
from the bottom declaration.

```
.rc:before {
  content: "";
  background-color: transparent;
  height: 200px;
  width: 250px;
  position: absolute;
  top: 20px;
  left: 320px;  
  
  box-shadow:
    /*screen*/
    -295px 20px 0 -20px black,
    -295px 20px 0 0 white;
}
```

<div class="drawing-container container-rc"><div class="rc rc-before-screen"></div></div>

The next step is creating the sides of the element, by overlaying white rectangles to
introduce negative space in the image.

```
.rc:before {
  content: "";
  background-color: transparent;
  height: 200px;
  width: 250px;
  position: absolute;
  top: 20px;
  left: 320px;

  box-shadow:
    /*left side*/
    -450px 185px 0 -85px white,
    -410px 160px 0 -85px white,
    -395px 160px 0 -85px white,

    /*right side*/
    -140px 185px 0 -85px white,
    -180px 160px 0 -85px white,
    -195px 160px 0 -85px white,

    /*screen*/
    -295px 20px 0 -20px black,
    -295px 20px 0 0 white;
}
```

<div class="drawing-container container-rc"><div class="rc rc-before-side"></div></div>

We're also going to use the `:before` element to create a key - as the base element
is a rectangle and resizing box shadows substracts the declared value from all sides,
we're going to first create a white rectangle with the right height for the key and
place the leftmost bottom key on the canvas. 

```
.rc:before {
  content: "";
  background-color: transparent;
  height: 200px;
  width: 250px;
  position: absolute;
  top: 20px;
  left: 320px;

  box-shadow:
    /*key*/
    -360px 245px 0 -87px white,
    
    /*left side*/
    -450px 185px 0 -85px white,
    -410px 160px 0 -85px white,
    -395px 160px 0 -85px white,

    /*right side*/
    -140px 185px 0 -85px white,
    -180px 160px 0 -85px white,
    -195px 160px 0 -85px white,

    /*screen*/
    -295px 20px 0 -20px black,
    -295px 20px 0 0 white;
}
```

<div class="drawing-container container-rc"><div class="rc rc-before-key"></div></div>

We can now add more keys by overlaying white and black rectangles of appropriate 
height and offsetting them by 25px in a grid, to create two rows of keys.

```
.rc:before {
  content: "";
  background-color: transparent;
  height: 200px;
  width: 250px;
  position: absolute;
  top: 20px;
  left: 320px;

  box-shadow:
    /*top row*/
    -160px 220px 0 -87px black,
    -185px 220px 0 -87px white,
    -210px 220px 0 -87px black,
    -235px 220px 0 -87px white,
    -260px 220px 0 -87px black,
    -285px 220px 0 -87px white,
    -310px 220px 0 -87px black,
    -335px 220px 0 -87px white,

    /*bottom row*/
    -185px 245px 0 -87px black,
    -210px 245px 0 -87px white,
    -235px 245px 0 -87px black,
    -260px 245px 0 -87px white,
    -285px 245px 0 -87px black,
    -310px 245px 0 -87px white,
    -335px 245px 0 -87px black,
    -360px 245px 0 -87px white,
    
    /*left side*/
    -450px 185px 0 -85px white,
    -410px 160px 0 -85px white,
    -395px 160px 0 -85px white,

    /*right side*/
    -140px 185px 0 -85px white,
    -180px 160px 0 -85px white,
    -195px 160px 0 -85px white,

    /*screen*/
    -295px 20px 0 -20px black,
    -295px 20px 0 0 white;
}
```

<div class="drawing-container container-rc"><div class="rc rc-before-keys"></div></div>

This leaves us with a residual black rectangle from overlaying the rightmost key
in the upper row, which we're going to overlay with a white shadow.

```
.rc:before {
  content: "";
  background-color: transparent;
  height: 200px;
  width: 250px;
  position: absolute;
  top: 20px;
  left: 320px;

  box-shadow:
    /*top row fix*/
    -20px 180px 0 0px white, 

    /*top row*/
    -160px 220px 0 -87px black,
    -185px 220px 0 -87px white,
    -210px 220px 0 -87px black,
    -235px 220px 0 -87px white,
    -260px 220px 0 -87px black,
    -285px 220px 0 -87px white,
    -310px 220px 0 -87px black,
    -335px 220px 0 -87px white,

    /*bottom row*/
    -185px 245px 0 -87px black,
    -210px 245px 0 -87px white,
    -235px 245px 0 -87px black,
    -260px 245px 0 -87px white,
    -285px 245px 0 -87px black,
    -310px 245px 0 -87px white,
    -335px 245px 0 -87px black,
    -360px 245px 0 -87px white,
    
    /*left side*/
    -450px 185px 0 -85px white,
    -410px 160px 0 -85px white,
    -395px 160px 0 -85px white,

    /*right side*/
    -140px 185px 0 -85px white,
    -180px 160px 0 -85px white,
    -195px 160px 0 -85px white,
    
    /*screen*/
    -295px 20px 0 -20px black,
    -295px 20px 0 0 white;
}
```

<div class="drawing-container container-rc"><div class="rc rc-before"></div></div>

For the last part of the image, we're going to use the `:after` pseudoelement, even 
though we could reuse the base shape from the `:before`. It will allow for adding a
simple animation to the image with just the elements we're going to animate - if we
put all of the code in the same pseudoelement, we'd need to repeat all the code in 
the animation.

Let's start with a base shape.

```
.rc:after {
  content: "";
  background-color: green;
  height: 200px;
  width: 200px;
  position: absolute;
  top: 20px;
  left: 320px;
}
```

<div class="drawing-container container-rc"><div class="rc rc-before rc-after-show"></div></div>

We're going to reuse this shape to overlay elements on the screen, in the same way we
did it for the keys, without the need to overlay green and black elements as the base
shape is a square.

```
.rc:after {
  content: "";
  background-color: transparent;
  height: 200px;
  width: 200px;
  position: absolute;
  top: 20px;
  left: 320px;

  box-shadow:
    /*top row*/
    -258px -20px 0 -87px #5dbd49,
    -310px -20px 0 -87px #5dbd49,
    -362px -20px 0 -87px #5dbd49,

    /*bottom row*/
    -235px 30px 0 -87px #5dbd49,
    -260px 30px 0 -87px #5dbd49,
    -312px 30px 0 -87px #5dbd49,    
    -337px 30px 0 -87px #5dbd49;
}
```

<div class="drawing-container container-rc"><div class="rc rc-before rc-after"></div></div>

Now we can add an animation to the `:after` layer without changing any of the other elements
in the image. We're going to use a simple `@keyframes` approach.

```
.rc:after {
  content: "";
  background-color: transparent;
  height: 200px;
  width: 200px;
  position: absolute;
  top: 20px;
  left: 320px;

  box-shadow:
    /*top row*/
    -258px -20px 0 -87px #5dbd49,
    -310px -20px 0 -87px #5dbd49,
    -362px -20px 0 -87px #5dbd49,

    /*bottom row*/
    -235px 30px 0 -87px #5dbd49,
    -260px 30px 0 -87px #5dbd49,
    -312px 30px 0 -87px #5dbd49,    
    -337px 30px 0 -87px #5dbd49;

  animation: pride 3s linear infinite;
}

@keyframes pride {
  0% {
    box-shadow:
    /*top row*/
    -258px -20px 0 -87px #5dbd49,
    -310px -20px 0 -87px #5dbd49,
    -362px -20px 0 -87px #5dbd49,

    /*bottom row*/
    -235px 30px 0 -87px #5dbd49,
    -260px 30px 0 -87px #5dbd49,
    -312px 30px 0 -87px #5dbd49,    
    -337px 30px 0 -87px #5dbd49; 
  }
  24% {
    box-shadow:
    /*top row*/
    -258px -20px 0 -87px #5dbd49,
    -310px -20px 0 -87px #5dbd49,
    -362px -20px 0 -87px #5dbd49,

    /*bottom row*/
    -235px 30px 0 -87px #5dbd49,
    -260px 30px 0 -87px #5dbd49,
    -312px 30px 0 -87px #5dbd49,    
    -337px 30px 0 -87px #5dbd49;
  }
  25% {
    box-shadow:
    /*top row*/
    -258px -20px 0 -87px #5dbd49,
    -310px -20px 0 -87px #4b9bde,
    -362px -20px 0 -87px #8f71b9,

    /*bottom row*/
    -235px 30px 0 -87px #e25786,
    -260px 30px 0 -87px #e25786,
    -312px 30px 0 -87px #f19e5a,    
    -337px 30px 0 -87px #f19e5a;
  }
  49% {
    box-shadow:
    /*top row*/
    -258px -20px 0 -87px #5dbd49,
    -310px -20px 0 -87px #4b9bde,
    -362px -20px 0 -87px #8f71b9,

    /*bottom row*/
    -235px 30px 0 -87px #e25786,
    -260px 30px 0 -87px #e25786,
    -312px 30px 0 -87px #f19e5a,    
    -337px 30px 0 -87px #f19e5a;
  }
  50% {
    box-shadow:
    /*top row*/
    -258px -20px 0 -87px #f19e5a,
    -310px -20px 0 -87px #5dbd49,
    -362px -20px 0 -87px #4b9bde,

    /*bottom row*/
    -235px 30px 0 -87px #8f71b9,
    -260px 30px 0 -87px #8f71b9,
    -312px 30px 0 -87px #e25786,    
    -337px 30px 0 -87px #e25786;
  }
  74% {
    box-shadow:
    /*top row*/
    -258px -20px 0 -87px #f19e5a,
    -310px -20px 0 -87px #5dbd49,
    -362px -20px 0 -87px #4b9bde,

    /*bottom row*/
    -235px 30px 0 -87px #8f71b9,
    -260px 30px 0 -87px #8f71b9,
    -312px 30px 0 -87px #e25786,    
    -337px 30px 0 -87px #e25786;
  }
  75% {
    box-shadow:
    /*top row*/
    -258px -20px 0 -87px #e25786,
    -310px -20px 0 -87px #f19e5a,
    -362px -20px 0 -87px #5dbd49,

    /*bottom row*/
    -235px 30px 0 -87px #4b9bde,
    -260px 30px 0 -87px #4b9bde,
    -312px 30px 0 -87px #8f71b9,    
    -337px 30px 0 -87px #8f71b9;
  }
  100% {
    box-shadow:
    /*top row*/
    -258px -20px 0 -87px #e25786,
    -310px -20px 0 -87px #f19e5a,
    -362px -20px 0 -87px #5dbd49,

    /*bottom row*/
    -235px 30px 0 -87px #4b9bde,
    -260px 30px 0 -87px #4b9bde,
    -312px 30px 0 -87px #8f71b9,    
    -337px 30px 0 -87px #8f71b9;
  }
}
```

<div class="drawing-container container-rc"><div class="rc rc-before rc-after-animated"></div></div>