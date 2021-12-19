---
title: Practising CSS - Cloning a website
date: 2020-08-21
tags: [css, learning, webdev]
---

![](https://res.cloudinary.com/davidsalter/image/upload/v1628542999/clone_py3lnh.jpg)

## Introduction

I've almost finished my second week of learning CSS, with the main (beginner) topics that are left are flexbox and responsive design. My plan is to get to them next week. To finish this week off though, I've decided to make a clone of the [Hyde Jekyll template](https://hyde.getpoole.com/).

This article is part of my 30 Days of CSS. You can follow my progress on Twitter at @cloudblogaas.

## Hyde

Hyde is a theme that can be used within Jekyll websites, the main feature being the strong left hand side bar (which can be on the right if you want) and clean content. You can see an image of it below.

![Alt Text](https://dev-to-uploads.s3.amazonaws.com/i/m8603o09i5fmn0emj390.png)

In making a copy of this, I've not looked at the CSS for the original (apart from obtaining the names of the fonts). I've tried to make a similar clone, but not an exact copy.

## Welcoming Seek - The Clone of Hyde

Looking at my clone below, you can see that its very similar. Its not a 100% copy, for example the padding is a bit different in places, but this is my first experience of copying a web site design and I'm very pleased with how it turned out.

![Alt Text](https://dev-to-uploads.s3.amazonaws.com/i/eqofaqtps8pb3t1ol14f.png)

I'm not going to go through the entire codebase of how I developed this, but here are some of the keypoints.

- To make things easier, I did all the HTML first and then styled it after.
- The layout is based on a floating column to the left of the screen.
- The floating column contains a div with a `position: fixed` attribute. This allows the left hand side to remain constant whilst the rest of the page is scrolled.
- The navigation components in the sidebar are all stored within a html `nav` element as a `ul`. From what I can read, this seems to be a standard way of doing navigation links.
- All the sizes are fixed and do not change when the page is resized. The page isn't responsive. This is on my list to implement.

You can get the full source code for this on my CodePen.

<p class="codepen" data-height="300" data-default-tab="html,result" data-slug-hash="QWNGrzj" data-user="cloudblogaas" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">
  <span>See the Pen <a href="https://codepen.io/cloudblogaas/pen/QWNGrzj">
  Seek</a> by davey (<a href="https://codepen.io/cloudblogaas">@cloudblogaas</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

## What did I learn doing this?

The main thing that I learned was that watching no amount of videos will make you good at CSS. You have to practice. There are no complicated images or effects in this page, it's more of an exercise in CSS layout.

You need to understand CSS Selector precedence. Doing this has shown me the sort of problems you can experience with css precedence if you're not careful with naming and ordering of elements.

Above all, I enjoyed doing this, and am looking forward to my next attempt.

## Credits

[Hyde Theme](https://hyde.getpoole.com/)

<span>Photo by <a href="https://unsplash.com/@bedeviere?utm_source=unsplash&amp;utm_medium=referral&amp;utm_content=creditCopyText">Bimata Prathama
