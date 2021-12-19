---
title: CSS Positioning
date: 2020-08-12
tags: [css, learning, webdev]
---

![](https://res.cloudinary.com/davidsalter/image/upload/v1628542805/30daysofcss_uuri6x.jpg)

## Introduction

To continue learning CSS, I've been looking at CSS positioning. This involves using the CSS `position` attribute which can be set to either `relative` or `absolute`.

`relative` means that element is positioned normally within the flow of the document relative to any other elements. After defining position as relative, an offset can be applied with the `top`, `bottom`, `left`, `right` attributes which define the position of the element relative to its normal position.

`absolute` means that the element is removed from the normal document flow and is positioned relative to the previous positioned ancestor, or if that doesn't exist, the top left of the page. As with `relative`, the attributes `top`, `bottom`, `left`, `right` can be applied.

## The Challenge

As a basic challenge, I've decided to write a web site "header" image. What I want is an image with a `h1` and `h2` text positioned on top of the image as shown in the following image.

![Web Page Header](https://dev-to-uploads.s3.amazonaws.com/i/t5ct2rop3ribponk9ver.png)

## The Solution

The HTML for this is fairly staightforward. As before, I've created a wrapper container and added the image and text to display. The wrapper class contains a div with a `heading` class that does most of the work.

```html
<!DOCTYPE html>
<html lang="en" dir="ltr">
  <head>
    <meta charset="utf-8" />
    <title></title>
    <link rel="stylesheet" href="style.css" />
  </head>

  <body>
    <div class="wrapper">
      <div class="heading">
        <img src="https://picsum.photos/id/551/800/300" alt="Site Logo" />
        <h1>Welcome To My World</h1>
        <h2>Developed By Me</h2>
      </div>
      <p>...</p>
      <p>...</p>
    </div>
  </body>
</html>
```

The `wrapper` class simply creates a centered block that is 80% the size of the page:

```css
.wrapper {
  width: 80%;
  margin: auto;
}
```

I then define the `heading` class, setting the `position`, `max-height` and `overflow` properties so that the background image is displayed correctly:

```css
.heading {
  position: relative;
  max-height: 180px;
  overflow: hidden;
}
```

I want the image to fill the entire width of its container, so I apply that via a CSS rule:

```css
.heading img {
  width: 100%;
}
```

I then define the position of the `h1` and `h2` elements as `absolute`, i.e. relative to the div with the `heading` class.

```css
.heading h1 {
  top: 0;
  width: 100%;
  position: absolute;
  text-align: center;
}

.heading h2 {
  top: 50px;
  width: 100%;
  position: absolute;
  text-align: center;
}
```

Finally, I want to bring all subsequent elements, for example the remainder of the page, back into the normal flow of the document. I do this by creating a `heading.after` style which is then applied after the heading is displayed.

```css
.heading:after {
  content: '';
  clear: both;
  display: block;
}
```

You can see the results on my CodePen below:

<p class="codepen" data-height="300" data-default-tab="html,result" data-slug-hash="BaKoJBQ" data-user="cloudblogaas" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">
  <span>See the Pen <a href="https://codepen.io/cloudblogaas/pen/BaKoJBQ">
  Site Header Image</a> by davey (<a href="https://codepen.io/cloudblogaas">@cloudblogaas</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

## Resources

Rather than adding an image to my project, I've used one from [Lorem Picsum](https://picsum.photos/).

To get dummy text to fill the page, I've used the [British Dummy Text Generator](https://www.thewebtaylor.com/tools/british-dummy-text-generator)

## Conclusion

Using positioning in CSS, its fairly straightforward to position elements on a page relative to other elements or the page itself. There's not much CSS in this sample, but I'm pleased with the results.

I'm continuing with my 30 Days Of CSS. You can follow my progress on Twitter @cloudblogaas.

## Credits

Photo by [Mike Lewis HeadSmart Media](https://unsplash.com/@mikeanywhere?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)
