---
title: Creating my logo animation
metaTitle: Creating an SVG path drawing animation.
metaDesc: Creating my logo animation using SVG and GreenSock
socialImage: /images/meta.jpg
image: /images/cassie.png
alt: "Cassie's logo"
date: '2019-07-30'
tags:
  - code
  - SVG
---

Last week I posted my new logo animation on [twitter](https://twitter.com/cassiecodes/status/1154650488681435137?ref_src=twsrc%5Etfw%7Ctwcamp%5Etweetembed%7Ctwterm%5E1154650488681435137&ref_url=http%3A%2F%2Flocalhost%3A8080%2Fposts%2Fcreating-my-logo-animation%2F%23heading-svg-stroke-dasharray).

Amongst everyone saying _a ton_ of lovely things, (thankyou) there was a resounding cry of "tutorial". So I'm going to try and break it down for you. Hope this helps someone, I had a ton of fun making it!

There's a few things going on in this logo animation. We've got -

- SVG stroke animation [jump to section >](#heading-svg-stroke-dasharray)
- SVG Clip-path [jump to section >](#heading-svg-lessclippathgreater)
- [GreenSock](https://greensock.com/) for the animation
- GreenSock's [Custom Bounce](https://greensock.com/wiggle-bounce), and [Draw SVG](https://greensock.com/drawSVG) plugins
- and a _whole_ load of drawing stuff out in illustrator.

---

I won't dive too much into GreenSock for this article. But as [Sara Soueidan](https://twitter.com/SaraSoueidan) has said

> GreenSock is the best thing that happened to SVG animations since SVG animations.

GreenSock provides better cross browser support for SVG animation than we get with CSS. It also, crucially, gives you the ability to chain animations and group animations on timelines. This is invaluable for longer and more complex animation.

They also have a bunch of fun plugins, some of which have been made specifically for SVG, like [Draw SVG](https://greensock.com/drawSVG) and [Morph SVG](https://greensock.com/morphSVG).

I've been side-eying their custom bounce plugin for a while, so when I saw an chance to use it to give the little dot some character I jumped (bounced? 😬) at the chance.

Although I love GreenSock, you don't need to learn a whole Javascript animation library to do SVG path animations.

We can do them with CSS too. So I'll run through a couple of different ways to create the same effect.

Let's get going. First up...

---

## SVG `stroke-dasharray`

`stroke-dasharray` is a SVG presentation attribute (which we can use as a CSS property) to make our SVG paths dashed instead of solid. The higher the number is, the the bigger the gap between dashes.

```svg
<path stroke-dasharray="10" ... />
```

```css
.dashedPath {
  stroke-dasharray: 10;
}
```

You can play around with what these values look like in this pen.

<p class="codepen" data-height="400" data-theme-id="0" data-default-tab="result" data-user="cassie-codes" data-slug-hash="EqZqXL" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="SVG stroke dasharray demo">
  <span>See the Pen <a href="https://codepen.io/cassie-codes/pen/EqZqXL/">
  SVG stroke dasharray demo</a> by Cassie Evans (<a href="https://codepen.io/cassie-codes">@cassie-codes</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

As well as making the dashes different lengths with `stroke-dasharray`, we can also offset the stroke position with `stroke-dashoffset`. If we change this property it looks like our dashes are moving along the path.

Like so...

<p class="codepen" data-height="400" data-theme-id="0" data-default-tab="result" data-user="cassie-codes" data-slug-hash="XvMraQ" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="SVG stroke dashoffset demo">
  <span>See the Pen <a href="https://codepen.io/cassie-codes/pen/XvMraQ/">
  SVG stroke dashoffset demo</a> by Cassie Evans (<a href="https://codepen.io/cassie-codes">@cassie-codes</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

If we make the gap between the dashes big enough and then change the offset we can create a path "drawing" effect.

<p class="codepen" data-height="400" data-theme-id="0" data-default-tab="result" data-user="cassie-codes" data-slug-hash="jgBNgd" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="SVG stroke dashoffset demo - animating">
  <span>See the Pen <a href="https://codepen.io/cassie-codes/pen/jgBNgd/">
  SVG stroke dashoffset demo - animating</a> by Cassie Evans (<a href="https://codepen.io/cassie-codes">@cassie-codes</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

Up until now we've been changing the value using a range input, but dashoffset and dasharray are animatable properties, so we can animate them with CSS like so -

<p class="codepen" data-height="265" data-theme-id="0" data-default-tab="css,result" data-user="cassie-codes" data-slug-hash="bd0b3722e1497cdfefdcf674a1b11598" data-preview="true" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="SVG stroke dashoffset demo - animated with CSS">
  <span>See the Pen <a href="https://codepen.io/cassie-codes/pen/bd0b3722e1497cdfefdcf674a1b11598/">
  SVG stroke dashoffset demo - animated with CSS</a> by Cassie Evans (<a href="https://codepen.io/cassie-codes">@cassie-codes</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

We can also use GreenSock's [draw svg plugin](https://greensock.com/drawSVG) to animate the stroke.

<p class="codepen" data-height="265" data-theme-id="0" data-default-tab="js,result" data-user="cassie-codes" data-slug-hash="6d0d3360d5801575db3f47e957fe2c67" data-preview="true" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="SVG stroke dashoffset demo - animated with GSAP">
  <span>See the Pen <a href="https://codepen.io/cassie-codes/pen/6d0d3360d5801575db3f47e957fe2c67/">
  SVG stroke dashoffset demo - animated with GSAP</a> by Cassie Evans (<a href="https://codepen.io/cassie-codes">@cassie-codes</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

Under the hood, this is how my logo animation works, but rather than having one continuous line I've broken the path up into nine separate sections. This gives me more control over the timing and helps to avoid any clipping overlaps, which we'll get to in a minute.

<p class="codepen" data-height="400" data-theme-id="0" data-default-tab="result" data-user="cassie-codes" data-slug-hash="85df19ba523672229f92e5aa4ac675af" data-preview="true" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="Cassie! - without clip paths - break down">
  <span>See the Pen <a href="https://codepen.io/cassie-codes/pen/85df19ba523672229f92e5aa4ac675af/">
  Cassie! - without clip paths - break down</a> by Cassie Evans (<a href="https://codepen.io/cassie-codes">@cassie-codes</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

Chaining animations in CSS is a bit of a nightmare as we have to do it with `animation-delay`.
With GreenSock, you can line these animations (or tweens) up on a timeline and easily tweak the timings of each tween in relation to the others.

You may have noticed that this version of my logo looks a little... messy though? SVG paths are a consistant width the whole way along. We can change the overall `stroke-width` and the shape of the `stroke-linecap` but we can't do much more than that.

Enter `<clipPath>`, we can use clip path to "cut out" a more stylised shape.

## SVG `<clipPath>`

SVG [clip path](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/clipPath) can be used to clip (or hide) parts of SVG elements according to a certain path. The parts of the shape inside the `<clipPath>` are visible, and the parts outside are hidden.

[This is a great CSS tricks article](https://css-tricks.com/masking-vs-clipping-use/) if you want to know more.

Lets go back to our little line animation.

In illustrator I drew out the path that we animated (purple), and then I drew a shape over the top (black). This will be used as a clip path.

<svg viewBox="0 0 403.44 74.33" xmlns="http://www.w3.org/2000/svg">
  <path d="M14.2 65.54s36-36.79 56.31-35.66 38.6 27.31 58.13 26.56 26.34-5.91 37.6-13 30.53-19.52 39.48-19.14 19.48 8.23 31.12 19.87 18.91 13.2 25.25 13.16S278.24 58 297 40.76s25.86-17.86 31.49-17.86 48.11 15.42 65 13.27" fill="none" stroke="#a387be" stroke-miterlimit="10" stroke-width="18"/>
  <path d="M40.88 48.72c2.09-1.26 4.21-2.4 6.33-3.57a61.49 61.49 0 0 1 16.3-6.58 32.75 32.75 0 0 1 10.73-.15 33.22 33.22 0 0 1 9.43 3.8c2.66 1.78 5.21 3.78 7.78 5.72 1.2.93 2.41 1.85 3.61 2.76a97.59 97.59 0 0 0 8.6 5.81 55 55 0 0 0 16 6.55 58.77 58.77 0 0 0 8.59 1.43 39.39 39.39 0 0 0 9.2-.42 66.56 66.56 0 0 0 8.96-2.07c1.11-.34 2.2-.79 3.3-1.18 2-.73 4-1.6 6-2.47a123.39 123.39 0 0 0 11.34-6c2.8-1.6 5.57-3.37 8.32-5.14l8.14-5.21c5.65-3.46 11.36-6.86 17.43-8.32a33.76 33.76 0 0 1 10.94.15 23.85 23.85 0 0 1 6 2.26c3.37 2.35 6.3 5.78 9.27 9.06 1.47 1.62 2.92 3.27 4.41 4.83 1.74 1.81 3.48 3.61 5.26 5.35a36.51 36.51 0 0 0 4.73 4 31.73 31.73 0 0 0 8.57 4.17 52 52 0 0 0 8.44 1.85 28.21 28.21 0 0 0 9.04-.35 46.66 46.66 0 0 0 8.76-2.45c1.42-.55 2.78-1.24 4.16-1.93L283.6 59c.93-.47 1.83-1.12 2.74-1.68l2.72-1.67C290.4 54.86 291.7 54 293 53c3.86-2.71 7.65-5.62 11.48-8.42 2-1.44 3.95-2.87 5.94-4.26 1.68-1.18 3.37-2.29 5.08-3.39a41.5 41.5 0 0 1 9.33-4.21 18.42 18.42 0 0 1 5 .19c2 .44 3.95 1 5.91 1.59s4.06 1.16 6.1 1.68c2.56.67 5.13 1.21 7.71 1.69a54.25 54.25 0 0 0 6.22.68 56.33 56.33 0 0 0 5.81-.11c2-.07 4-.39 6-.62 4-.45 7.9-1.29 11.82-2.08l11.07-2.28c-3.92-.95-7.82-1.95-11.73-2.93-3.62-.9-7.24-1.75-10.85-2.69-4-1-8-1.94-11.92-3.27-1.08-.37-2.15-.72-3.23-1.07s-2.1-.86-3.15-1.28c-2-.79-3.91-1.67-5.87-2.49-1.72-.73-3.44-1.52-5.19-2.11-1.18-.4-2.36-.78-3.55-1.15a30.15 30.15 0 0 0-7.24-1.27h-.8a20.44 20.44 0 0 0-8.27 2.07c-1.05.47-2.08 1-3.13 1.53s-1.8 1.07-2.69 1.6c-1.2.71-2.38 1.46-3.55 2.28-7.3 5.03-14.3 11.09-21.52 16.24-1.18.84-2.35 1.72-3.56 2.47l-3.55 2.19-4.53 2.3a38 38 0 0 1-9 3.16 28.53 28.53 0 0 1-9.22.07 37.35 37.35 0 0 1-10.37-3.62 45 45 0 0 1-7.3-6.36 336.02 336.02 0 0 1-5.15-5.48l-4.93-5.43a42.6 42.6 0 0 0-6.17-5.71 24.46 24.46 0 0 0-8.47-4 40.78 40.78 0 0 0-8.75-1.2 32.93 32.93 0 0 0-9.67 1.27 51.07 51.07 0 0 0-8.3 3.44c-1 .46-1.94 1.07-2.91 1.6-.74.41-1.49.81-2.22 1.26-2.2 1.33-4.41 2.65-6.6 4Q173 31.54 169 34.09c-10.14 6-20.46 11.87-31.35 14.23a42.57 42.57 0 0 1-7.2.66 37.68 37.68 0 0 1-7-.69 58.59 58.59 0 0 1-17.59-7.06c-3.4-2.17-6.7-4.67-10-7.13l-3.44-2.57a57.38 57.38 0 0 0-6.48-4.08 31.29 31.29 0 0 0-7.7-3 27.64 27.64 0 0 0-9.78-.73 34.81 34.81 0 0 0-13.73 5l-2.89 1.75c-1.28.78-2.51 1.7-3.75 2.6-1.83 1.31-3.59 2.76-5.36 4.24-3.65 3-7.14 6.48-10.63 9.9-.54.54-1.06 1.18-1.58 1.78q-1.91 2.18-3.8 4.39c-1.19 1.38-2.37 2.78-3.55 4.18-1.41 1.68-2.74 3.49-4.09 5.27 1.56-1.09 3.11-2.24 4.69-3.27 1.21-.77 2.41-1.56 3.61-2.36 2.19-1.45 4.38-2.93 6.55-4.42 2.3-1.28 4.63-2.66 6.95-4.06z" fill="#171717"/>
</svg>

This is what the syntax for a clip path looks like in SVG

```svg
<svg>
  <clipPath id="myClipPath">
    <circle cx="40" cy="35" r="35" />
  </clipPath>

  <path clip-path="url(#myClipPath)"... />
</svg>
```

Anything you put inside the clip path element will be used as a clipping object.
You reference a clip path on the clipping target using an ID

You can also reference a clip path in CSS like this:

```css
.element {
  clip-path: url('#myClipPath');
}
```

This is what the line animation looks like with a clip path applied. Much Nicer!

<p class="codepen" data-height="265" data-theme-id="0" data-default-tab="result" data-user="cassie-codes" data-slug-hash="0cdbc3747ec7c7873758bc215bc6873c" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="SVG stroke dashoffset demo - clip-path">
  <span>See the Pen <a href="https://codepen.io/cassie-codes/pen/0cdbc3747ec7c7873758bc215bc6873c/">
  SVG stroke dashoffset demo - clip-path</a> by Cassie Evans (<a href="https://codepen.io/cassie-codes">@cassie-codes</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

For my logo animation I created a clip path for each stroke.

<svg id="Layer_1" data-name="Layer 1" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1398.79 415.58">
  <g id="strokes" fill="none" stroke="#bc85ff" stroke-linecap="round" stroke-linejoin="round" stroke-width="79">
    <path id="c-2" d="M253.9 133.68s-1.73-34-36.73-41.59-67.58 8-76.25 50.6S116 246 143.35 279.23s72.08 43 124.76 14.56 75.2-107.43 84.89-138.62 38.81-79 123-45.75"/>
    <path id="a1-2" d="M344.35 182.55s-11.09 85.25 6.24 94.26 33.27 10.74 37.43 6.24 95.65-94.61 118.52-144.86 19.06-62 19.06-65.5"/>
    <path id="a2-2" d="M524.56 72s-39.85 178.12-38.81 190.6-8.67 70.7 11.09 69.31 60.3-39.51 64.45-55.1"/>
    <path id="s1-2" d="M689.17 122.94s-11.78-42.63-35.69-47.13-42.28-4.81-55.1 8.31-30.15 47.83 2.08 76.25 74.16 47.47 85.25 76.24 15.94 72.08 9.36 83.86-21.84 31.19-37.07 30.85-40.54-6.93-51.63-20.45a129.85 129.85 0 0 1-16.64-26.34"/>
    <path id="s2-2" d="M615 337.46s-17-22.53-15.94-41.24 19.41-57.19 27.72-66.89 54.07-38.47 67.24-48.86 45.39-33.27 49.9-47.83 26-50.6 35.69-54.06 41.59-17 57.19-2.43 33.27 41.59 33.27 45.75"/>
    <path id="s3-2" d="M860.72 103.18s-14.56-21.83-28.07-28.07-42.28-4.85-55.1 8.32-30.15 47.83 2.07 76.24 74.17 47.48 85.26 76.25 15.94 72.08 9.35 83.86-21.83 31.22-37.08 30.85-40.54-6.94-51.63-20.45"/>
    <path id="s4-2" d="M791.41 339.54a49.25 49.25 0 0 1-15.94-34.66c0-21.49 14.2-57.53 22.52-69s21.14-36.74 65.85-46.79S954.64 160 963 150.32s18.72-24.61 18.72-43"/>
    <path id="i-2" d="M979.93 130.56s-11.78 87.33-6.58 109.17 3.81 72.77 24.95 73.12 62.7-12.48 69.7-41.24c0-29.8 27.72-110.9 42.28-127.19s37.43-51.29 71-48.86 58.22 18.71 58.57 32.58 3.81 46.78-22.87 71.39-45.4 33.61-57.18 31.53"/>
    <path id="e-2" d="M1112 146.16s-17.67 94.61-11.78 112.28 23.91 57.53 60.65 55.1 82.82-17 103.27-53.37 40.2-67.92 42.28-78.32"/>
  </g>
  <g id="clips" fill="#fff" stroke="#232323" stroke-miterlimit="10" stroke-width="3">
    <path id="c" d="M491.82 85c11.78 9.71 20.45 25.65 13.17 40.91s-29.12 26.69-35.36 17.33a32.9 32.9 0 0 0-21.14-16c-14.56-3.81-38.48 3.12-54.77 29.47s-15.26 38.48-21.5 54.08-10.74 37.78-45.75 70-82.85 45.41-98.8 48.18a211.06 211.06 0 0 1-39.17 3.82l-3.47.34a21.32 21.32 0 0 0-6.58 0l-.7-1h-1l-2.77.35c-.35 0-.69 0-.69-.35h-.7c-29.46-3.81-50.61-19.41-63.43-47.14a166.45 166.45 0 0 1-13.18-42.29 225.86 225.86 0 0 1-2.08-43.33 14.14 14.14 0 0 1 .35-3.82 11.3 11.3 0 0 0 0-4.85c1.73-13.52 3.81-26.69 6.59-39.52a152.39 152.39 0 0 1 13.86-38.13c3.82-6.93 8-13.52 12.48-20.1a94.57 94.57 0 0 1 16-17.34C157 62.83 174.63 55.55 193.7 54.16c18-1 36.05 4.51 54.42 16.64a61.33 61.33 0 0 1 23.23 32.58c1 4.86 2.08 9.71 2.77 14.22.69 5.2 1 10.05 1.39 15.25a30.22 30.22 0 0 1-1.39 12.48 29.17 29.17 0 0 1-6.59 11.44 16.37 16.37 0 0 1-15.59 7.28A18.62 18.62 0 0 1 237.38 154a66.5 66.5 0 0 1-5.55-11.1c-1.39-3.81-2.77-8-4.16-12.13-4.16-11.09-16.29-16.29-27.39-12.13a21.21 21.21 0 0 0-6.58 4.16 50.25 50.25 0 0 0-10.4 14.9 213.71 213.71 0 0 0-16.29 60 211.52 211.52 0 0 0 2.08 62.75 30.92 30.92 0 0 0 11.09 20.1 43.44 43.44 0 0 0 22.18 8.32 69 69 0 0 0 49.58-14.56 127.52 127.52 0 0 0 40.9-47.14c8.67-18.72 8.67-25.31 13.87-46.8s11.78-40.9 22.53-55.81S362.52 74.61 393 66.29s49.23-4.16 68.64 2.08S480 75 491.82 85z" opacity=".59"/>
    <path id="a1" d="M533.07 142.9c4.85-10.05 9.36-20.45 9.7-33.28s.35-12.82-2.07-19.06a22.81 22.81 0 0 0-20.46-15.25 20.56 20.56 0 0 0-12.82 2.77c-3.47 2.08-11.44 14.21-12.83 17s-25.65 48.87-30.85 56.5-25.31 34.32-39.17 50.26-44.37 48.88-47.84 47.49-9.71-10-2.77-44-44.72-23.92-58.24 1.39.69 76.26 4.85 85.62 23.23 21.11 37.43 23.54 28.43-2.77 41.95-12.13c4.5-3.12 9-6.59 13.17-10.05 3.81-3.47 44-42.3 67.59-72.45s47.51-67.95 52.36-78.35z" opacity=".48"/>
    <path id="a2" d="M556 278.79c-14.22-4.85-25.31 4.51-29.81 11.09a39.61 39.61 0 0 1-9.36 10.75q-2.61 2.08-6.24 5.2c-.35-3.12.69-26 1.38-32.59a192.46 192.46 0 0 1 3.03-19.41c3.47-17 7.28-34 10.75-51q5.72-25.47 11.44-51c1.39-7.28 2.43-14.22 3.47-21.5s1.73-14.21 2.42-21.14A35.46 35.46 0 0 0 541 90.21 22.82 22.82 0 0 0 520.59 75a20.66 20.66 0 0 0-12.83 2.77c-3.46 2.08-6.58 6.59-12.82 17s-25 99.49-29.12 120.63a436.41 436.41 0 0 0-7.28 60c0 7.63 0 15.26.35 22.54a69 69 0 0 0 7.62 27.38 50.17 50.17 0 0 0 13.87 19.07 42.84 42.84 0 0 0 18.72 9 41 41 0 0 0 20.45-2.08 48.07 48.07 0 0 0 19.76-13.52 171.94 171.94 0 0 0 17.33-24.27c3.47-6.65 13.52-30.22-.64-34.73z" opacity=".6"/>
    <path id="s1" d="M710.55 211.54c-.69-1.39-18.72-29.12-40.21-41.25a284.08 284.08 0 0 1-25.65-15.95c-8-5.2-16.64-11.09-25-17.33-2.77-2.08-22.53-22.88-9.36-36.74s35-4.86 39.52-2.43a36.07 36.07 0 0 1 11.79 11.09 33.65 33.65 0 0 0 4.16 5.2c1.38 1.73 3.12 3.47 4.5 5.2a14.35 14.35 0 0 0 14.91 4.16A14.51 14.51 0 0 0 696 111.7a25.84 25.84 0 0 0-1-19.06 79.27 79.27 0 0 0-15.59-23.92A67.17 67.17 0 0 0 644 47.92 65 65 0 0 0 603.44 51a77.26 77.26 0 0 0-33.63 26.69C561.15 89.87 557 98.88 557 116.56s4.5 36.74 17 50.61 36.06 28.77 53.39 47.49S654.4 243.78 662 276s-9 45.06-16.29 46.45a18.16 18.16 0 0 1-19.07-10.06c-2.42-3.46-5.2-6.23-15.94-10.39a18.39 18.39 0 0 0-23.23 11.78 10.31 10.31 0 0 0-.69 3.12 33.53 33.53 0 0 0 3.14 20.1 70.64 70.64 0 0 0 28.77 29.12 63.89 63.89 0 0 0 39.87 6.93 65.73 65.73 0 0 0 35.35-13.87 80.91 80.91 0 0 0 23.23-29.81c17.68-39.14 15.6-78.66-6.59-117.83z" opacity=".64"/>
    <path id="s2" d="M601 353.67L630.13 318a23.89 23.89 0 0 1-5.55-6.24 43.72 43.72 0 0 1-5.89-12.13 37.32 37.32 0 0 1-.69-13.87 111.89 111.89 0 0 1 8.66-25.65c3.82-8.32 9-18.37 17-25s47.49-32.58 57.54-37.78 34.67-16.3 49.92-38.83 14.88-45.06 41.24-60.31 45.76 7.27 51.31 13.86 11.78 18 25.65 9.71 6.59-27 3.47-32.24A75.53 75.53 0 0 0 845.4 58c-19.4-12.85-53.73-16.67-77.65 1-23.92 17.34-32.58 44.72-43 62.05s-28.08 36.4-48.53 46.46-39.18 26.34-48.54 33.27a110.74 110.74 0 0 0-20.79 21.5A163.32 163.32 0 0 0 593 244.82a168.22 168.22 0 0 0-10.4 24.61 76.94 76.94 0 0 0 5.2 66.9 91.37 91.37 0 0 0 13.2 17.34z" opacity=".63"/>
    <path id="s3" d="M890.12 211.54c-.69-1.39-18.72-29.12-40.21-41.25a282.84 282.84 0 0 1-25.66-15.95c-8-5.2-16.63-11.09-25-17.33-2.78-2.08-22.54-22.88-9.36-36.74s35-4.86 39.51-2.43a36 36 0 0 1 11.79 11.09 34.25 34.25 0 0 0 4.16 5.2c1.39 1.73 3.12 3.47 4.51 5.2a14.32 14.32 0 0 0 14.9 4.16 14.51 14.51 0 0 0 10.75-11.79 25.89 25.89 0 0 0-1-19.06 79.11 79.11 0 0 0-15.6-23.92 67.13 67.13 0 0 0-35.36-20.8A65.05 65.05 0 0 0 783 51a77.23 77.23 0 0 0-33.62 26.69c-8.67 12.14-12.83 21.15-12.83 38.83s4.51 36.74 17 50.61 36.05 28.77 53.38 47.49 27 29.12 34.67 61.36-9 45.06-16.3 46.45a18.14 18.14 0 0 1-19.06-10.06c-2.43-3.46-5.2-6.23-16-10.39a18.38 18.38 0 0 0-23.22 11.78 10 10 0 0 0-.7 3.12 33.53 33.53 0 0 0 3.16 20.12 70.6 70.6 0 0 0 28.78 29.12 63.86 63.86 0 0 0 39.86 6.93 65.73 65.73 0 0 0 35.36-13.87 81 81 0 0 0 23.22-29.78c17.68-39.17 15.6-78.69-6.58-117.86z" opacity=".67"/>
    <path id="s4" d="M780.58 353.67L809.7 318a23.89 23.89 0 0 1-5.55-6.24 43.44 43.44 0 0 1-5.89-12.13 37.53 37.53 0 0 1-.7-13.87 131.74 131.74 0 0 1 8.67-25.65c3.81-8.32 9-18.37 17-25S852 205 882.15 198.71a364.48 364.48 0 0 0 71.41-22.88c22.18-9.35 46.79-29.46 51.3-46.1s-8.67-36.4-34-32.93c-25.65 8-17.68 34.32-37.79 52s-48.18 19.76-73.83 22.88-42.64 22.53-52 29.46a111 111 0 0 0-20.8 21.49 163.92 163.92 0 0 0-13.87 22.53 169.9 169.9 0 0 0-10.4 24.62 77 77 0 0 0 5.2 66.9 158.09 158.09 0 0 0 13.21 16.99z" opacity=".72"/>
    <path id="i" d="M1131.39 241.35c-12.48-6.93-6.59-29.81 1-32.58s25.65-4.16 41.94-14.56 26.35-19.76 39.17-38.48-3.12-34.67-16.64-34.67-25 1.39-40.9 18.72-18.72 28.43-28.77 53.73-23.92 60-36.4 79.39a220.4 220.4 0 0 1-31.89 39.86c-5.2 4.85-19.76 12.83-26 16.29-22.88 12.48-47.49 7.28-64.13-11.78a85.71 85.71 0 0 1-13.87-21.15 128.9 128.9 0 0 1-8.32-22.88 312 312 0 0 1-9.36-66.9 322.45 322.45 0 0 1 5.2-68.29 63.86 63.86 0 0 1 2.47-11.05 45 45 0 0 1 4.85-11.1C957 100.26 968.11 93 983 94.37a30.26 30.26 0 0 1 17.33 8.32 37.8 37.8 0 0 1 10.4 19.76 96.37 96.37 0 0 1 3.47 26.35c-.69 12.48-1.73 24.61-3.12 36.74s-2.43 24.27-3.12 36.75c-.35 6.93-1 13.86-1.39 20.79-.69 6.94-1.38 13.52-2.08 20.11v6.24a17.67 17.67 0 0 0 1 5.89 24.16 24.16 0 0 0 3.82 8c2.08 2.77 6.24 2.77 11.78-.7h.7l.69-.69 1-1c.69 0 1 0 1-.35s.35-.34.69-.34l2.78-2.78 4.16-4.16a77.23 77.23 0 0 0 18-37.43c3.47-18.72 1.39-32.94 16.29-75.23s58.59-79.38 84.59-88.39 67.59-8.32 90.47 15.94 28.08 50.61 22.53 67.6-12.48 38.48-50.26 68.64-69.86 23.85-82.34 16.92z" opacity=".64"/>
    <path id="e" d="M1317.54 155.38a18.19 18.19 0 0 0-13.52-.69 22.45 22.45 0 0 0-11.79 9.36c-2.42 4.16-4.5 8.32-6.58 11.78s-3.82 7.63-5.9 11.79l-10 20.45c-3.47 6.59-6.59 13.18-10 19.76A174.27 174.27 0 0 1 1242 252.1a121.28 121.28 0 0 1-22.53 19.41 87.69 87.69 0 0 1-24.27 11.79 78.88 78.88 0 0 1-27 2.42 42.51 42.51 0 0 1-13.52-3.12 27.21 27.21 0 0 1-10.74-8 41.3 41.3 0 0 1-8.67-13.17c-2.08-5.2-7.63-11.44-3.81-46.11 0-44 16.64-64.47 6.24-72.79-39.17-7.28-54.43 36.74-69 70.37s3.47 74.87 6.94 79.38a126.23 126.23 0 0 0 18.37 20.1 98.58 98.58 0 0 0 48.18 20.8 127.83 127.83 0 0 0 56.16-2.08 132 132 0 0 0 49.22-26 209.54 209.54 0 0 0 48.19-53.74 355.47 355.47 0 0 0 31.54-65.51 10.61 10.61 0 0 0 .7-2.78 9.18 9.18 0 0 1 1-4.16c2.06-10.01-1.76-18.33-11.46-23.53z" opacity=".6"/>
  </g>
</svg>

So the end result looks like this!

<p class="codepen" data-height="400" data-theme-id="0" data-default-tab="result" data-user="cassie-codes" data-slug-hash="942590678837398a4390506983586e59" data-preview="true" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="Cassie! - with clip paths - break down">
  <span>See the Pen <a href="https://codepen.io/cassie-codes/pen/942590678837398a4390506983586e59/">
  Cassie! - with clip paths - break down</a> by Cassie Evans (<a href="https://codepen.io/cassie-codes">@cassie-codes</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

---

## The duotone effect

I've created the duotone effect by animating two paths instead of one along each section, one purple and one green.

These paths are grouped together in a `<g>` element which is the target for the clipping area.

<p class="codepen" data-height="265" data-theme-id="0" data-default-tab="js,result" data-user="cassie-codes" data-slug-hash="0809d0f2b8d0d253625ced675a6f3fbe" data-preview="true" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="SVG stroke dashoffset demo - Stagger - animated with GSAP">
  <span>See the Pen <a href="https://codepen.io/cassie-codes/pen/0809d0f2b8d0d253625ced675a6f3fbe/">
  SVG stroke dashoffset demo - Stagger - animated with GSAP</a> by Cassie Evans (<a href="https://codepen.io/cassie-codes">@cassie-codes</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

## Squash and stretch.

The final cherry on top is the little dot on the i. 💚

In order to make a realistic bounce, the element needs to abide by the [squash and stretch](https://blog.animationmentor.com/squash-and-stretch-the-12-basic-principles-of-animation/) animation principle.

This helps make the movement feel more lifelike. The i should squash and stick to the ground at the bottom of the bounce and stretch out at the top. You can definitely achieve this with some really fine tuned keyframes or individual, overlapping tweens. But GreenSock make it easier for us with their [Custom Bounce](https://greensock.com/wiggle-bounce) plugin.

Using the plugin you set a few parameters and it creates an ease for the bounce and for the squash and stretch.

`strength` determines how bouncy the ease is and `squash` determines how long the squash should last.

```js
CustomBounce.create('myBounce', {strength: 0.7, squash: 3, squashID: 'myBounce-squash'});
```

You can then use that bounce ease the same way you would use a normal ease in a GreenSock tween, by referring to it in your tween parameters.
The squash ease ID will be whatever the ID of the bounce is plus `-squash` appended to the end, for example, `ease:"myBounce-squash"`

```js
tl.to('#ball', duration, {y: 550, ease: 'myBounce'}).to(
  '#ball',
  duration,
  {scaleY: 0.5, scaleX: 1.3, ease: 'myBounce-squash'},
  0
);
```

<p class="codepen" data-height="600" data-theme-id="0" data-default-tab="result" data-user="cassie-codes" data-slug-hash="MNpVxy" data-preview="true" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="Video: CustomBounce from GreenSock">
  <span>See the Pen <a href="https://codepen.io/cassie-codes/pen/MNpVxy/">
  Video: CustomBounce from GreenSock</a> by Cassie Evans (<a href="https://codepen.io/cassie-codes">@cassie-codes</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

## The final animation

Put all of that together along with a bit of timing finetuning and easing tweaks and we have our final logo animation!

<p class="codepen" data-height="400" data-theme-id="0" data-default-tab="result" data-user="cassie-codes" data-slug-hash="mNWxpL" data-preview="true" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="Cassie!">
  <span>See the Pen <a href="https://codepen.io/cassie-codes/pen/mNWxpL/">
  Cassie!</a> by Cassie Evans (<a href="https://codepen.io/cassie-codes">@cassie-codes</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

If you make an SVG path animation I would love to see it! My twitter DM's are also always open if you get stuck.

[Just pop me a message!](https://twitter.com/cassiecodes)
