---
layout: post
title:      "Animation in CSS"
date:       2021-04-12 01:28:22 -0400
permalink:  animation_in_css
---


This week, I had attended an online event "Think like an Animator: Applying Traditional Animation Principles to CSS" by Devon Anderson.  The event was inspiring to me.  Anderson was an animator who worked for TV companies like Warner Brosmentor.  Now, he is a Senior UI Engineer at LinkedIn.  He applied 12 principles of animation to create animations in web applications.  The advantages of creating own animations can be:
* improving loading time of web applications
* better feedback
* directs attention of viewers

In this event, he also showed some examples created by CSS rules and elements.

One of the ways to create an animation is using **@keyframes**  with animation properties.
```
@keyframes example {
  from {top: 0px;}
  to {top: 200px;}
}
```
```
@keyframes animationname {keyframes-selector {css-styles;}}
```
**@keyframes** is to set the rules of animation movements. * animationname* defines the name of the animation.
*keyframes-selector* is the animation duration and it can set legal values as 0-100% or from ... to ...

```
div {
  width: 100px;
  height: 100px;
  background: red;
  position: relative;
  animation: example 5s infinite;
}
``` 
Then, it creates a 100X100 red rectangle.  **animation** is to set the frequency of animation movement.  
Under animation, there are properties to make more complex animations.
* animation-name: specifies the names of one or more @keyframes
* animation-duration: sets the length of time that an animation takes to complete one cycle.
* animation-delay:  specifies the amount of time to wait.
* animation-iteration-count: sets the number of times an animation sequence should be played before stopping.
* animation-direction: sets whether an animation should play forward, backward, or alternate back and forth between playing the sequence forward and backward.
* animation-timing-function: sets how an animation progresses through the duration of each cycle.
* animation-fill-mode: sets how an animation applies styles to its target before and after its execution.

Another way is using **transform** with hover. 

```
transform: none|transform-functions|initial|inherit;
```
The transform properties:
```
div {
  transform: rotate(20deg);
}
```
rotate(any_deg) rotates an element clockwise or counter-clockwise according to a given degree.
```
div {
  transform: translate(50px, 100px);
}
```
translate(x-coordinate, y-coordinate) moves an element from its current position.
```
div {
  transform: scale(2, 3);
}
```
scale(sx, sy) increases or decreases the size of an element.
```
div {
  transform: skew(20deg, 10deg);
}
```
skew(x_deg, y_deg) skews an element along the X and Y-axis by the given angles.
```
div {
  transform: matrix(1, -0.3, 0, 1, 0, 0);
}
```
matrix(a, b, c, d, tx, ty) combines all the 2D transform methods into one. *"a b c d"*: describing the linear transformation.  *"tx ty"* describing the translation to apply.

To sum up, applying traditional animation principles to CSS can make websites had better loading time and look better.  To achieve that, we need to go deep into the animation and transform properties in CSS.  


