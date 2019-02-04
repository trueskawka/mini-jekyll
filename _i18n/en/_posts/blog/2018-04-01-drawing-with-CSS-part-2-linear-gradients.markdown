---
layout: post_default
title:  "Drawing with CSS - part 2: linear gradients"
date:   2018-04-01 16:55:00 +4:00:00
categories: ["blog", "programming", "css"]
author: "alicja"
published: true
---
The first part of the drawing with CSS tutorial was all about 
[box shadows]({{ "/blog/programming/2018/02/07/drawing-with-CSS-part-1-box-shadows.html" | prepend: site.baseurl_root  }}).
They are pretty flexible when it comes to creating multiple rectangular and circular shapes, but there are some
limitations.

Three main disadvantages of using only box shadows for drawing:
- their rotation is the same as the rotation of the parent element
- their size is defined relative to the parent element
- there is a limit of three base shapes

In this blog post I want to focus on another tool for drawing with CSS - gradients. Gradients are images created with a 
function. In their most basic form, they are a progressive transition between two colors. Let's take an initial look as 
to what that means, starting with a simple black square.

<link rel="stylesheet" href="{{ "/css/linear-gradients.css" | prepend: site.baseurl_root  }}">

```
.boxy {
  height: 100px;
  width: 100px;
  
  background: black;
}
```

<div class="drawing-container"><div class="boxy"></div></div>

Now we can add different transitions using a simple linear gradient:

```
/* From top to bottom */
.boxy {
  height: 100px;
  width: 100px;

  background: linear-gradient(to bottom, black, white);
}
```

<div class="drawing-container"><div class="boxy boxy-gradient-bottom"></div></div>

```
/* From bottom to top */
.boxy {
  height: 100px;
  width: 100px;
  
  background: linear-gradient(to top, black, white);
}
```

<div class="drawing-container"><div class="boxy boxy-gradient-top"></div></div>

```
/* From left to right */
.boxy {
  height: 100px;
  width: 100px;
  
  background: linear-gradient(to right, black, white);
}
```

<div class="drawing-container"><div class="boxy boxy-gradient-right"></div></div>

```
/* From right to left */
.boxy {
  height: 100px;
  width: 100px;
  
  background: linear-gradient(to left, black, white);
}
```

<div class="drawing-container"><div class="boxy boxy-gradient-left"></div></div>

So how are gradients defined? Because they are images created with a function, they are applied to 
the `background` property, more specifically `background-image`. Before we go any further, let's take a closer 
look at the some of the `background` properties:
- `background-color` (keyword/RGB/HSL) - sets the background color of an element
- `background-image` - sets one or more background images for an element, including 
  gradients
- `background-position` (keyword/px/em/percentage) - sets the position of the background
- `background-repeat` (keyword) - defines how background images are repeated
- `background-size` (keyword/px/em/percentage) - specifies the size of a background element
- `background-attachment` (keyword) - defines the scrolling beahviour

Most of the time there is no need to set all of those properties at once and it's possible to
use a `background` shorthand to concisely define only those that are needed. All the other properties
will be set to their initial values.

The `background` shorthand is defined as:
```
background: <attachment> <image> <position> <size> <repeat>;
```
and it can have multiple layers. The layers are positioned as a stack from the bottom-most declaration - the 
lowest layer is going to be at the bottom of the stack and the subsequent layers are going to cover
it - which we can use for multi-layer drawings!

Linear gradients are defined with the `linear-gradient()` function that we can use in the `background-image`
property. Let's dive into the properties of that function:
-  `<side-or-corner>` or `<angle>` - the first property of a linear gradient defines where the transition 
  between colors starts and ends, which can be defined either by keywords (`to bottom`, `to right`) or angles
  (`180deg`, `90deg`)
- two or more `<color-stop>` values - each has a color value (keyword/RGB/HSL) followed by an optional `stop` 
  position (percentage/px/em), they define what colors and at what points are transitioning

The description on its own might not be entirely clear, so let's look at some examples. To better understand 
the angle of a linear gradient, you can play with the fiddle below - it's a linear gradient transitioning 
between three colors: red, pink and orange (the middle pink color is used to visualize the gradient line better).

<div id="gradient-container">
  <div id="gradient-box">
    <div id="js-gradient">
      <div id="perpendicular-line"></div>
      <div id="gradient-line"></div>
    </div>
  </div>
  <div id="angle-form">
    <h4>Angle <span id="ang">45</span>&deg;</h4>
    <input 
          name="angle" type="range" 
          min="0" max="360" 
          value="45" step="1"
          id="angle" />
  </div>
</div>

This gradient is defined as follows:
```
linear-gradient(
  <angle> deg,
  red 0%,
  #ff9a9e 50%, #ff9a9e 50%,
  orange 100%
);
```

1. The `<angle>` value is taken directly from the value in the fiddle.
2. The first color-stop declaration `red 0%` means that color `red` is the first color and it starts at the 
  very beginning of the gradient line at `0%`. Technically, the `0%` here is superfluous, as it's the initial 
  value for the first color-stop.
3. The second and third color-stop declarations define the pink strip - it starts and stops at `50%`. The first 
  color-stop is linearly transitioning from being pure `red` to being more and more `#ff9a9e`. Then the third
  color-stop is linearly transitioning from being pure `#ff9a9e` to being more and more `orange` - the last color
  stop.
4. The last color-stop declaration defines the orange part of the gradient - it goes from where the last color-stop
  ends at `50%` to the end of the gradient line at `100%`. Technically `100%` here is superfluous, as it's the initial
  value for the last color-stop.

A few things worth mentioning at this point:
- CSS gradients have no intrinsic dimensions - they define a color progression within the element they are applied to 
  and don't have a preferred size nor a preferred ratio. They repeat so as to fill the container. You can imagine them
  as stripes of color progressively transitioning from the first color to the last, placed along the gradient line.
- CSS function calculates the color values in a way that makes the corners the full defined color - the starting points 
  start at a perpendicular line between the corner and the gradient line - I found the [MDN visualization](https://developer.mozilla.org/en-US/docs/Web/CSS/linear-gradient#Composition_of_a_linear_gradient)
  useful for understanding how it works.
- Color-stops can have values below `0%` and above `100%` - since linear gradients don't have an intrinsic size and are 
  defined along a gradient line, the stop values can be placed at any point along this line. 

By now you probably understand pretty well how to use gradients as backgrounds, but how can they be used for drawing? 
Let's gather some useful insights here:
- an element can have multiple background images that can be layered on top of each other
- each background image can have a precisely defined size (its bounding box) and position for the upper left corner
- linear gradients can be defined with either percentages or pixels and can be used as background images

Before we get started with drawing, let's explore the last point a bit more - a linear gradient can be defined using 
either percentages or lenghts. 

The first gradient below is defined using percentages and the second one is using pixels. Initially, the angle is `0deg`
and they both look the same. But changing the angle is going to either change the width of the line (percentage gradient)
or the position (pixel gradient). Depending on which variable we want to control, it's sensible to use an appropriate 
approach for linear gradients. 

```
/* Percentages */
linear-gradient(
  <angle> deg,
  white 0%, white 40%,
  orange 40%, orange 60%,
  white 60%, white 100%
);

/* Pixels */
linear-gradient(
  <angle> deg,
  white 0%, white 80px,
  orange 80px, orange 120px,
  white 120px, white 100%
);
```

<div id="gradient-container2">
  <div class="gradient" id="percentage-gradient"></div>
  <div class="gradient" id="pixel-gradient"></div>
  <div id="angle-form2">
    <h4>Angle <span id="ang2">0</span>&deg;</h4>
    <input name="angle2" type="range" min="0" max="360" id="angle2" value="0" />
  </div>
</div>

Now we have all the tools to draw something using linear gradients. Let's work through an example, recreating the
[Haskell](https://www.haskell.org/) logo. It consists of a few simple linear elements and we're going to start from
the lower right part of the lambda as the base element. If you look at the image closely, you can see that the same
element is repeated throughout the lambda and the preceeding chevron, simply mirrored at different angles.

```
.haskell {
  width: 156px;
  height: 144px;
  
  color: orange;
  
  background:
    linear-gradient(
      237.5deg,
      transparent 77px,
      currentColor 77px,
      currentColor 129px,
      white 129px
    );
}
```

<div class="container-haskell"><div class="haskell-lower-right"></div></div>

Next let's mirror the lower right part to create the bottom part of the lambda. We need more fine-grained 
control for positioning the elements, so let's set the size of each background element to `156` by `144` 
pixels with `background-size` and make sure the background doesn't repeat itself. Now we're going to use
an initial position for each background element, to make sure it's in the right place, and double the width
of the image.

```
.haskell {
  width: 312px;
  height: 144px;
  
  color: orange;
  
  background:
    linear-gradient(
      122.5deg,
      transparent 80px,
      currentColor 80px,
      currentColor 132px,
      transparent 132px
    ),
    linear-gradient(
      237.5deg,
      transparent 77px,
      currentColor 77px,
      currentColor 129px,
      white 129px
    ) 156px 0;
  
  background-size: 156px 144px;
  background-repeat: no-repeat;
}
```

<div class="container-haskell"><div class="haskell-lower-lambda"></div></div>

Let's add the upper part of the lambda - we need to move elements around a bit, to make sure they overlap properly.
Also, now the image needs double the single element height.

```
.haskell {  
  width: 312px;
  height: 288px;
  
  color: orange;
  
  background:
    linear-gradient(
      237.5deg,
      transparent 77px,
      currentColor 77px,
      currentColor 129px,
      transparent 129px
    ) 65px 0,
    linear-gradient(
      122.5deg,
      transparent 80px,
      currentColor 80px,
      currentColor 132px,
      transparent 132px
    ) 65px 144px,
    linear-gradient(
      237.5deg,
      transparent 77px,
      currentColor 77px,
      currentColor 129px,
      white 129px
    ) 156px 144px;
  
  background-size: 156px 144px;
  background-repeat: no-repeat;
}
```

<div class="container-haskell"><div class="haskell-lambda"></div></div>

The next element of the logo is a chevron on the left side of the lambda. It's made of the same base 
elements as the lambda, just mirrored a bit. Let's increase the size of the image, move the lambda a bit
to the right and add a black chevron.

```
.haskell {
  width: 345px;
  height: 288px;
  
  color: orange;
  
  background:
    /* black chevron */
    /* upper part */
    linear-gradient(
      237.5deg,
      transparent 77px,
      #101c24 77px,
      #101c24 129px,
      transparent 129px
    ) 0 0,
    /* lower part */
    linear-gradient(
      122.5deg,
      transparent 80px,
      #101c24 80px,
      #101c24 132px,
      transparent 132px
    ) 0 144px,

    /* lambda */
    /* upper part */
    linear-gradient(
      237.5deg,
      transparent 77px,
      currentColor 77px,
      currentColor 129px,
      transparent 129px
    ) 85px 0,
    /* lower left part */
    linear-gradient(
      122.5deg,
      transparent 80px,
      currentColor 80px,
      currentColor 132px,
      transparent 132px
    ) 85px 144px,
    /* lower right part */
    linear-gradient(
      237.5deg,
      transparent 77px,
      currentColor 77px,
      currentColor 129px,
      white 129px
    ) 176px 144px;
  
  background-size: 156px 144px;
  background-repeat: no-repeat;
}
```

<div class="container-haskell"><div class="haskell-chevron"></div></div>

The last part of the image are two parallel lines on the right. We need to add a bit of space to
fit the lines to the right and add two linear gradients with either `0deg` or `180deg`.

```
.haskell {
  width: 380px;
  height: 288px;
  
  color: orange;
  
  background:
    /* black chevron */
    /* upper part */
    linear-gradient(
      237.5deg,
      transparent 77px,
      #101c24 77px,
      #101c24 129px,
      transparent 129px
    ) 0 0,
    /* lower part */
    linear-gradient(
      122.5deg,
      transparent 80px,
      #101c24 80px,
      #101c24 132px,
      transparent 132px
    ) 0 144px,

    /* lambda */
    /* upper part */
    linear-gradient(
      237.5deg,
      transparent 77px,
      currentColor 77px,
      currentColor 129px,
      transparent 129px
    ) 85px 0,
    /* lower left part */
    linear-gradient(
      122.5deg,
      transparent 80px,
      currentColor 80px,
      currentColor 132px,
      transparent 132px
    ) 85px 144px,
    /* lower right part */
    linear-gradient(
      237.5deg,
      transparent 77px,
      currentColor 77px,
      currentColor 129px,
      white 129px
    ) 176px 144px,
    
    /* black lines */
    /* upper line */
    linear-gradient(
      180deg,
      white 80px,
      #101c24 80px,
      #101c24 132px,
      white 132px
    ) 220px 0px,
    /* lower line */
    linear-gradient(
      180deg,
      white 12px,
      #101c24 12px,
      #101c24 64px,
      white 64px
    ) 220px 144px;
  
  background-size: 156px 144px;
  background-repeat: no-repeat;
}
```

<div class="container-haskell"><div class="haskell-with-lines"></div></div>

Now it's almost done - we just need to add two more white base elements between the orange lambda
and the black lines to visually separate them.

```
.haskell {
  width: 380px;
  height: 288px;
  
  color: orange;
  
  background:
    /* black chevron */
    /* upper part */
    linear-gradient(
      237.5deg,
      transparent 77px,
      #101c24 77px,
      #101c24 129px,
      transparent 129px
    ) 0 0,
    /* lower part */
    linear-gradient(
      122.5deg,
      transparent 80px,
      #101c24 80px,
      #101c24 132px,
      transparent 132px
    ) 0 144px,

    /* lambda */
    /* upper part */
    linear-gradient(
      237.5deg,
      transparent 77px,
      currentColor 77px,
      currentColor 129px,
      transparent 129px
    ) 85px 0,
    /* lower left part */
    linear-gradient(
      122.5deg,
      transparent 80px,
      currentColor 80px,
      currentColor 132px,
      transparent 132px
    ) 85px 144px,
    /* lower right part */
    linear-gradient(
      237.5deg,
      transparent 77px,
      currentColor 77px,
      currentColor 129px,
      white 129px
    ) 176px 144px,
    
    /* white spacer */
    /* upper part */
    linear-gradient(
      237.5deg,
      transparent 77px,
      white 77px,
      white 129px,
      transparent 129px
    ) 110px 0,
    /* lower part */
    linear-gradient(
      237.5deg,
      transparent 80px,
      white 80px,
      white 132px,
      transparent 132px
    ) 206px 144px,
    
    /* black lines */
    /* upper line */
    linear-gradient(
      180deg,
      white 80px,
      #101c24 80px,
      #101c24 132px,
      white 132px
    ) 220px 0px,
    /* lower line */
    linear-gradient(
      180deg,
      white 12px,
      #101c24 12px,
      #101c24 64px,
      white 64px
    ) 220px 144px;
  
  background-size: 156px 144px;
  background-repeat: no-repeat;
}
```

<div class="container-haskell"><div class="haskell"></div></div>

You can edit this drawing interactively on [CodePen](https://codepen.io/trueskawka/pen/pLVGgx?editors=1100).

<script src="{{ "/scripts/linear-gradients.js" | prepend: site.baseurl_root }}"></script>