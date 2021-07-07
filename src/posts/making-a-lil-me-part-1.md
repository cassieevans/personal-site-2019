---
title: Making lil' me - part 1.
metaTitle: Making a lil' me - Part 1.
metaDesc: How to get values from mouse movement and plug them into an animation - the mouse movement bit.
socialImage: /images/avatar.jpg
image: /images/avatar.jpg
alt: "Cassie's logo"
date: '2020-06-30'
tags:
  - code
  - SVG
---

I'm so excited to have _finally_ launched my website.

This was one of my favourite bits to make **by far**. A little animated me that responds to the cursor position.

<p class="codepen" data-height="580" data-theme-id="dark" data-preview="true" data-default-tab="result" data-user="cassie-codes" data-slug-hash="WNQqZJG" style="height: 582px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="lil' me.">
  <span>See the Pen <a href="https://codepen.io/cassie-codes/pen/WNQqZJG">
  lil' me.</a> by Cassie Evans (<a href="https://codepen.io/cassie-codes">@cassie-codes</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

In this post we'll cover how to get values from the mouse movement and plug them into an animation.

This is my **favourite** thing about SVG animation. Not mouse movement in particular, but interactivity. Exporting animation as an mp4 is cool and all. But you can't _play_ with it.

Animating on the web opens up so many cool possibilities!

If you're the kind of person who learns best by digging into the code, then just go ahead and fork that pen, have fun!

---

Still here? 😊 Cool beans, let's go through it together.

I use [GreenSock](https://greensock.com/) for all my SVG animation. There are a few reasons for this. Firstly, it gives you more control and is super fun to use. But more importantly, as browsers implement the SVG spec differently, animating can get a bit messy. The team at GreenSock put a lot of effort into harmonizing behavior across browsers.

Under the hood though, GreenSock is just a really performant property manipulator. It will manipulate (pretty much) whatever value you want to change. In this case it's updating the values in a matrix transform. Check this out.

![gif of matrix transforms updating with dev tools open](/images/cursor.gif)

Cool right! GreenSock is great at this kind of thing, but there's a lot going on in this pen, so let's break it down into chunks and focus on one bit at a time. First up, the mouse interaction. We'll come back to GreenSock in a bit!

## Mouse movement fun!

So, there's another way we can modify values in CSS with Javascript. [CSS custom properties!](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_custom_properties)

<p class="codepen" data-height="500" data-theme-id="dark" data-default-tab="result" data-user="cassie-codes" data-slug-hash="OJMOLML" style="height: 422px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="Mouse movement demo">
  <span>See the Pen <a href="https://codepen.io/cassie-codes/pen/OJMOLML">
  Mouse movement demo</a> by Cassie Evans (<a href="https://codepen.io/cassie-codes">@cassie-codes</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

Let's walk through the process. The goal is to get the x and y position of the cursor as we move our mouse about, and then we can use those values to update some custom properties.

In the previous gif GreenSock is using matrix transforms, which are more performant. But we're going to use `transform: translate(x,y)` for this as it's a tad more readable, and we can use percentage values.

First up, we declare some custom properties for the x and y values and then use those custom properties in place of regular property values in our transform.

```css
:root {
  --mouse-x: 0;
  --mouse-y: 0;
}

.pointer {
  transform: translate(var(--mouse-x), var(--mouse-y));
}
```

Then we need to get our mouse coordinates.
We can do this by listening to the mouse movement and getting the mouse's coordinates with `clientX` and `clientY`

We'll assign those values to some global variables so that we can access them in another function later.

```js
let xPosition;
let yPosition;

// updating the mouse coordinates
function updateMouseCoords(event) {
  xPosition = event.clientX;
  yPosition = event.clientY;
}
window.addEventListener('mousemove', updateMouseCoords);
```

Now we've got our coordinates, lets use them to update our custom properties and move our pointer around!

😇 When animating things on the web it's important to pay attention to performance. We can't know for sure how many times a users mouse event will fire as it's implementation specific. If it fires more than 60 times per second (which is the average refresh rate for most browsers) we'll end up with unnecessary computation.
So to avoid burning up everyone's laptop fans and melting their CPU we can use `requestAnimationFrame`

Request animation frame allows us to make smoother, browser optimized animations. If the user changes tabs, the animation will stop and give the CPU a break. You call the function once to start, and then it recusively calls itself.

⚠️ This is a great way to crash your browser if you get some code wrong!

```js
function movePointer() {
  // update the CSS vars!
  requestAnimationFrame(movePointer);
}
requestAnimationFrame(movePointer);
```

Right now, the x and y position we're getting back from `event.clientX` and `event.clientY` are pixel values, this isn't really useful to us.
If we plug them straight into our custom properties we could end up transforming our pointer hundreds of pixels off in one direction.

<p class="codepen" data-height="500" data-theme-id="dark" data-default-tab="result" data-user="cassie-codes" data-slug-hash="5ed677605b34f86eeb488b4b2a9bfe6a" style="height: 504px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="Mouse movement demo">
  <span>See the Pen <a href="https://codepen.io/cassie-codes/pen/5ed677605b34f86eeb488b4b2a9bfe6a">
  Mouse movement demo</a> by Cassie Evans (<a href="https://codepen.io/cassie-codes">@cassie-codes</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

We need to convert those pixel values into percentages of the window height and width.

Let's define some global variables for the window width and height and work out the percentage values.

```js
let windowHeight = window.innerHeight;
let windowWidth = window.innerWidth;

function percentage(partialValue, totalValue) {
  return (100 * partialValue) / totalValue;
}

function movePointer() {
  x = percentage(xPosition, windowWidth);
  y = percentage(yPosition, windowHeight);

  window.requestAnimationFrame(movePointer);
}
requestAnimationFrame(movePointer);
```

This will give us a range from 0 - 100, which would just move our pointer downwards and to the right, so we can shift the range by subtracting 50.

Once we've got our nice centralised range, we can plug it into our custom properties using `setProperty()`

```js
let windowHeight = window.innerHeight;
let windowWidth = window.innerWidth;

function percentage(partialValue, totalValue) {
  return (100 * partialValue) / totalValue;
}

function movePointer() {
  // shift the range from 0 to 100 to -50 to 50
  // this will keep the movement centralised
  x = percentage(xPosition, windowWidth) - 50;
  y = percentage(yPosition, windowHeight) - 50;

  // update the custom properties
  document.documentElement.style.setProperty('--mouse-x', `${x}%`);
  document.documentElement.style.setProperty('--mouse-y', `${y}%`);

  window.requestAnimationFrame(movePointer);
}
requestAnimationFrame(movePointer);
```

The user might resize the browser, so it's good to check for that.

```js
// update if browser resizes
function updateWindowSize() {
  windowHeight = window.innerHeight;
  windowWidth = window.innerWidth;
}
window.addEventListener('resize', updateWindowSize);
```

We can also avoid unnecessary calculations when the mouse position hasn't changed by storing past x and y positions and comparing them to the new positions.

```js
let xPosition;
let yPosition;

let storedXPosition;
let storedYPosition;

function movePointer() {
  window.requestAnimationFrame(movePointer);
  // only recalculating if the value changes
  if (storedXPosition === xPosition && storedYPosition === yPosition) return;

  // update custom properties

  // update the stored positions with the current positions
  storedXPosition = xPosition;
  storedYPosition = yPosition;
}
```

😇 another _super important_ bit is checking whether the user has a preference set in their OS for reduced motion.
Some people have vestibular disorders and get mega quesy when looking at animations.

I pop this check at the start of all my animation functions, and return out if necessary.

```js
const safeToAnimate = window.matchMedia('(prefers-reduced-motion: no-preference)')
  .matches;

if (!safeToAnimate) return;
```

<p class="codepen" data-height="500" data-theme-id="dark" data-preview="true" data-default-tab="result js" data-user="cassie-codes" data-slug-hash="OJMOLML" style="height: 422px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="Mouse movement demo">
  <span>See the Pen <a href="https://codepen.io/cassie-codes/pen/OJMOLML">
  Mouse movement demo</a> by Cassie Evans (<a href="https://codepen.io/cassie-codes">@cassie-codes</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

## What about GreenSock?!

Let's see how we can do the same thing using GreenSock!
First we have to include the core library, if you're on codepen, you can go to your pen settings and search for greensock.

![pen settings on codepen](/images/gsap-include.png)

Now we've got the GSAP library in our pen, we can take advantage of `gsap.ticker()`

The greensock ticker is like the heartbeat of the GSAP engine - under the hood it uses requestAnimationFrame, just like we were using, but if that isn’t supported, the ticker automatically falls back to using a regular setTimeout() loop.

So rather than calling our function with request animation frame, we would do this...

```js
function movePointer() {
  //animation code
}
// gsap's RAF, falls back to set timeout
gsap.ticker.add(movePointer);
```

Aside from making structuring animation itself more intuitive, GreenSock provides a load of super cool utility functions that make your life easier.
Remember all that work we did to get a nice usable range? Chuck that all in the bin. Look what we can do now!

```js
// Set up our coordinate mapping with GSAP utils!
let mapWidth;
let mapHeight;
function setMaps() {
  mapWidth = gsap.utils.mapRange(0, innerWidth, -50, 50);
  mapHeight = gsap.utils.mapRange(0, innerHeight, -50, 50);
}
window.addEventListener('resize', setMaps);
setMaps();
```

Now we can listen to the mouse movement, feed `clientX` and `clientY` into mapWidth, and we'll get back a value within the range we've set!

```js
let xPosition;
let yPosition;

// updating the mouse coordinates
function updateMouseCoords(event) {
  xPosition = mapWidth(event.clientX);
  yPosition = mapWidth(event.clientY);
}
window.addEventListener('mousemove', updateMouseCoords);
```

So tidy and concise!

Instead of updating CSS custom properties, we're going to use a GreenSock tween.
A Tween is what does all the animation work, as I said at the start, it's like a high-performance property manipulator.

A tween takes in two parameters

The targets, which are the object(s) whose properties you want to animate. GreenSock uses `document.querySelectorAll()` internally so we can use any CSS selector we would use in CSS, or a direct reference to an element.

And the vars, an object containing all the properties/values you want to animate, along with any special properties like delay, ease, or duration.

```js
function movePointer() {
  gsap.to(pointer, {
    xPercent: xPosition,
    yPercent: yPosition,
    ease: 'none'
    // ease: 'power4.out',
    // ease: 'power4.in'
  });
}
// gsap's RAF, falls back to set timeout
gsap.ticker.add(movePointer);
```

I've added a couple more easing equations in here so you can change them out and see what a difference it can make to the movement.

<p class="codepen" data-height="400" data-theme-id="dark" data-preview="true" data-default-tab="js,result" data-user="cassie-codes" data-slug-hash="rNxYpzO" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="Mouse movement demo - GSAP - easing">
  <span>See the Pen <a href="https://codepen.io/cassie-codes/pen/rNxYpzO">
  Mouse movement demo - GSAP - easing</a> by Cassie Evans (<a href="https://codepen.io/cassie-codes">@cassie-codes</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

If you want to focus more on performance than easing you can use gsap.quickSetter to update the transforms.
Creating a quickSetter function can boost performance around 50% - 250%!

```js
// Use .quickSetter for best performance since we're updating values a lot dynamically
//apply it to the pointer x/y properties and append a "%" unit
const xSet = gsap.quickSetter(pointer, 'x', '%');
const ySet = gsap.quickSetter(pointer, 'y', '%');

function movePointer() {
  xSet(xPosition);
  ySet(yPosition);
}
// gsap's RAF, falls back to set timeout
gsap.ticker.add(movePointer);
```

<p class="codepen" data-height="400" data-theme-id="dark" data-preview="true" data-default-tab="js,result" data-user="cassie-codes" data-slug-hash="oNboYNy" style="height: 422px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="Mouse movement demo - GSAP">
  <span>See the Pen <a href="https://codepen.io/cassie-codes/pen/oNboYNy">
  Mouse movement demo - GSAP</a> by Cassie Evans (<a href="https://codepen.io/cassie-codes">@cassie-codes</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

Thanks for reading!

Next up - lets apply this to a SVG and fake some three dimensional movement!

(check back in for the next article soon!)

Edit - 30/07/2020 - it's on it's way! I just got a little busy

Edit - 01/01/2021 - Yep. Note to self. Don't try to do 2 part blog post series during a pandemic.

Got any questions about this article? [Just pop me a message!](https://twitter.com/cassiecodes)
