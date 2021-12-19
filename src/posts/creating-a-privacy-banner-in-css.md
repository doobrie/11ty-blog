---
title: Creating a Privacy Banner in CSS
date: 2020-08-14
tags: [css, learning, webdev]
---

![](https://res.cloudinary.com/davidsalter/image/upload/v1628542805/30daysofcss_uuri6x.jpg)

## Introduction

Continuing my 30 Days of CSS, I've decided to spend more time on positioning to make sure that I understand it fully. I've been learning CSS for about a week now and am finding it gradually becoming more natural :)

You can follow my progress on Twitter @cloudblogaas

## The Challenge

Today's challenge was to build a privacy notification bar that's locked to the bottom of the browser window. I'm not worried about any of the JavaScript, or the notification displaying every time I load the page. The challenge here is purely about CSS.

![Notification Bar](https://dev-to-uploads.s3.amazonaws.com/i/ho0r41td8zcmzhqyntbo.png)

## The Solution

As usual, the HTML is pretty simple. It consists of a single `div` containing a single `p` and single `button`.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link rel="stylesheet" href="style.css" />
    <title>Privacy Banner</title>
  </head>
  <body>
    <p>...</p>
    <p>...</p>
    <p>...</p>
    <p>...</p>

    <div class="notification">
      <p>
        This site uses cookies to understand how you use the site and improve your
        experience. This includes personalizing content and advertising. By continuing to
        use this site, you agree to this use.
      </p>
      <button>Learn More</button>
    </div>
  </body>
</html>
```

The containing `div` for the notification bar is defined as:

```css
.notification {
  position: fixed;
  left: 0px;
  bottom: 0px;
  background: #646464;
  transition: 1000ms;
}
```

This defines the position as `fixed` and on the bottom of the page. The reason this is set to `fixed` and not `absolute` is so that the notification does not move when the page is scrolled.

A new attribute here is `transition`, that makes the notification bar "fade in" to the page within a time of 1000ms. This just makes it look a little nicer.

The paragraph inside the notification simply has padding and color defined:

```css
.notification p {
  padding: 20px 20px 0px;
  color: #ffffff;
}
```

Similarly, the button has styling to change it from looking like a basic button to a more styled button.

```css
.notification button {
  background-color: rgb(52, 102, 187);
  color: #ffffff;
  padding: 10px 50px;
  margin: 10px 10px;
}
```

Finally, I've added a `hover` attribute onto the button to slightly change the color when the mouse is moved over it.

```css
.notification button:hover {
  background-color: rgb(103, 138, 187);
}
```

You can see the final version in my CodePen:

<p class="codepen" data-height="300" data-default-tab="html,result" data-slug-hash="qBZZWYq" data-user="cloudblogaas" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">
  <span>See the Pen <a href="https://codepen.io/cloudblogaas/pen/qBZZWYq">
  privacy-banner</a> by davey (<a href="https://codepen.io/cloudblogaas">@cloudblogaas</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>
