---
title: CSS layout using grids
date: 2020-08-19
tags: [css, learning, webdev]
---

![](https://res.cloudinary.com/davidsalter/image/upload/v1628542805/30daysofcss_uuri6x.jpg)

## Introduction

I'm currently completing 30 Days of CSS. Last week I looked at [creating a simple blog layout](https://davidsalter.com/posts/css-layout/) using CSS layout with `float: left` and `float:right`. Now that I've started learning about CSS grids, I thought I'd try to replicate the solution with a CSS grid.

## The Solution

When creating a simple blog layout, I've used the same HTML code.

```html
<div class="wrapper">
  <div class="title">
    <h1>Minimal Blog Page Layout</h1>
  </div>
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

There is nothing layout related within this HTML, that is all held within the CSS. CSS Grid Layout works by specifying the `display:grid` property on a "parent" element. All of the "child" elements are then laid out in a grid pattern. The really nice feature of CSS Grid though, is that each item placed on the grid can take up and number of rows and columns in the grid and can be positioned anywhere on the grid.

The layout I want to create is shown in the image below.

![Grid Layout](https://dev-to-uploads.s3.amazonaws.com/i/2qv3tz8pcp21w8mfj3cb.png)

In this layout, there is a grid of 4 columns and 2 rows. To create the blog, I want the title to cover all of row 1. I want the main content to cover row 2, columns 1 to 3. Finally, I want the sidebar to cover row 2, column 4.

To start creating this layout, I needed to specify the grid layout in the `.wrapper` div:

```css
.wrapper {
  max-width: 80%;
  margin: auto;
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr;
  column-gap: 10px;
}
```

The `display: grid` property tells CSS that I want to use a grid. The `grid-template-columns: 1fr 1fr 1fr 1fr` property says that I want to create 4 columns, each with a width of `1fr`. This means a with of 1 fraction of the total width. Since there are 4 columns, that means each column will be 25% of the width of the wrapper. So, the main column with be 75% width of the wrapper, and the sidebar will be 25% of the width of the wrapper.

Finally, the `column-gap: 10px` property states that there will be a 10px gap between all of the columns.

Next, I need to define how the HTML is displayed within the grid.

### The Title

The title is enclosed in a div with class `.title`

```css
.title {
  grid-column-start: 1;
  grid-column-end: 5;
}
```

This CSS says that the `.title` class starts at grid column line 1, and ends at grid column line 5. This therefore fills all 4 columns of the grid. As this is the first HTML within the grid, this will take up all of the first row of the grid.

### The Main Column

The main column is enclosed in a div with the class `.mainColumn`

```css
.mainColumn {
  grid-column-start: 1;
  grid-column-end: 4;
}
```

This CSS states that this class will fill the first 3 columns of the grid, from column row 1 through column row 4. As this HTML is placed directly after the title (which fills and entire row), the main column will be placed starting at the left and taking up the first 3 columns of the row.

### The Sidebar

Finally, the sidebar is defined in a class called `.sidebar`, very similar to the main column:

```css
.sidebar {
  grid-column-start: 4;
  grid-column-end: 5;
}
```

This CSS states that the sidebar will fill the final column of the row (as it will be placed after the main column) and will be placed between column lines 4 and 5.

You can see the finished example in my CodePen:

<p class="codepen" data-height="300" data-default-tab="html,result" data-slug-hash="wvGzyRr" data-user="cloudblogaas" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">
  <span>See the Pen <a href="https://codepen.io/cloudblogaas/pen/wvGzyRr">
  Minimal Blog Layout Using Grid</a> by davey (<a href="https://codepen.io/cloudblogaas">@cloudblogaas</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

## Resources

Net Ninja YouTube series on CSS Grid Tutorial

<ResponsiveVideo url='https://www.youtube.com/embed/x7tLPhnA06w' />

## Summary

I really like the CSS Grid layout. It's much easier and more intuitive than using `float:left` or `float:right`. There's plenty more that it can do that I'm looking forward to learning and practising.

## Credits

Photo by [Mike Lewis HeadSmart Media](https://unsplash.com/@mikeanywhere?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)
