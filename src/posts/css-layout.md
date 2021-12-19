---
title: CSS Layout
date: 2020-08-11
tags: [css, learning, webdev]
---

![](https://res.cloudinary.com/davidsalter/image/upload/v1628542805/30daysofcss_uuri6x.jpg)

## Introduction

Today is day 4 of my 30 Days of CSS, and I've been learning about CSS layouts using "float". This seems like a good start to me and should form a good foundation that I can expand upon and learn CSS Grid and Flexbox in the coming days. You can follow my progress on Twitter @cloudblogaas

## Today's Problem

To challenge myself, I've decided that I should try and made a basic two column blog layout using CSS.

## The Solution

The first step to developing this layout, is to add a wrapper class so that I can place the blog in the center of the page. This is done using the following CSS which sets the width of the wrapper to 80% of the page width and centers it automatically.

```css
.wrapper {
  max-width: 80%;
  margin: auto;
}
```

To implement the columns within the wrapper, I've basically created 2 classes:

```css
.mainColumn {
  float: left;
  width: 75%;
  background: red;
}
.sidebar {
  float: right;
  width: 25%;
  background: yellow;
}
```

The `.mainColumn` class is defined as 75% of the width of its container and is floated to the left. The `.sidebar` container is 25% of the container and is floated to the right.

I've then added a bit of styling to add a margin and separate the layout slightly.

```css
.panel {
  margin: 10px;
  background: white;
}

.mainColumn p,
.mainColumn h1 {
  margin: 10px;
  line-height: 1.5;
  background: white;
}
```

The HTML for the page is basically a few divs to give me the wrapper and columns that I want.

```html
<div class="wrapper">
  <h1>Minimal Blog Page Layout</h1>
  <div class="mainColumn">
    <h1>Heading</h1>
    <p>...</p>
    <p>...</p>
    <p>...</p>
  </div>
  <div class="sidebar">
    <div class="panel">
      <h2>Heading</h2>
      <p>...</p>
    </div>
    <div class="panel">
      <h2>Heading</h2>
      <p>...</p>
    </div>
    <div class="panel">
      <h2>Heading</h2>
      <p>...</p>
    </div>
  </div>
</div>
```

And that's it. The main work here is the div's that `float:left` and `float:right`.

You can see a working version of the page on my CodePen:

<p class="codepen" data-height="300" data-default-tab="html,result" data-slug-hash="wvGKgqM" data-user="cloudblogaas" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">
  <span>See the Pen <a href="https://codepen.io/cloudblogaas/pen/wvGKgqM">
  Minimal Blog Page Layout</a> by davey (<a href="https://codepen.io/cloudblogaas">@cloudblogaas</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

## Resources

I've been following a series on YouTube by The Net Ninja on CSS Positioning:

<ResponsiveVideo url='https://www.youtube.com/embed/7ZXsPj43heo' />

## Credits

Photo by [Mike Lewis HeadSmart Media](https://unsplash.com/@mikeanywhere?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)
