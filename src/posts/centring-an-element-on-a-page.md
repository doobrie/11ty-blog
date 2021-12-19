---
title: Centring an element on a page
date: 2020-09-07
tags: [css, learning, webdev]
---

![](https://res.cloudinary.com/davidsalter/image/upload/v1628542805/30daysofcss_uuri6x.jpg)

There are no doubt multiple ways of centring an element on a page using CSS, but here I present 2 different techniques that are straightforward and easy to implement. You could use these techniques for centring a login box, or an information message, etc. in the middle of a page.

![Vertically and Horizontally Centred divs](https://dev-to-uploads.s3.amazonaws.com/i/zndbi6mhsc0esyd16r2f.png)

## Implementation

To center a couple of `div`s, I've defined them on the page as follows:

```html
<div class="centered1">I am centered div 1.</div>
<div class="centered2">I am centered div 2.</div>
```

There's nothing special about these `div`s apart from they have the classes `centered1` and `centered2` respectively.

### Technique 1

The class `.centered1` uses an absolute position for the `div` and sets it to display 50% from the top and 50% from the left of the page. The margin of the `div` is then set to `margin: -200px 0 0 -250px;`. Since the `div` is 500px across and 400px wide, this margin has the effect of moving the div 250px left and 200px up, making it display in the center of the page.

```css
.centered1 {
  position: absolute;
  width: 500px;
  height: 400px;
  top: 50%;
  left: 50%;
  background-color: goldenrod;
  margin: -200px 0 0 -250px;
}
```

### Technique 2

The second technique is almost the same as the first, but this time I haven't used the `margin` to position the `div` in the middle of the page.

As previously, I've set the position to `absolute` and given the `div` a width and height. I've set the `top` and `left` properties to 50% to move the top left corner of the `div` to the centre of the screen.

Finally, I use a transformation to move the center of the `div` to be on the center of the page. `transform: translate(-50%, -50%);`. Again, the result is that the component is displayed in the center of the screen.

```css
.centered2 {
  position: absolute;
  width: 300px;
  height: 200px;
  top: 50%;
  left: 50%;
  background-color: red;
  transform: translate(-50%, -50%);
}
```

## Further Thoughts

Here, I've shown two simple techniques. I could probably have created the same effect using FlexBox or a Grid layout, or probably a number of other ways. If there are any different techniques that you use, please leave a comment below.

## Credits

Photo by [Mike Lewis HeadSmart Media](https://unsplash.com/@mikeanywhere?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)
