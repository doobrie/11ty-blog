---
title: Dock Flickering In Gnome
date: 2021-02-24
tags: [linux, gnome]
featured_image: gnome.jpg
---

![](/images/gnome.jpg)
I'm using Pop!\_OS on my current machine, and have the Gnome desktop enabled.

I'm always on the lookout for good tweaks and one that I like is [Dash To Dock](https://extensions.gnome.org/extension/307/dash-to-dock/) that turns the standard Dashboard along the left hand side of the screen into a dock, similar to on MacOS. I really like this extension as it fits my workflow better to have a dock on my screen that automatically hides when a window covers it. You can add your favourite applications to it and also add the application launcher.

![Dash To Dock](https://extensions.gnome.org/extension-data/screenshots/screenshot_307_VW5dorQ.png)

I'm currently running Gnome version 3.38.3 and there's a [small bug](https://github.com/micheleg/dash-to-dock/issues/1359) that makes the application launcher flicker when its opened.

Fortunately, there is a simple fix described [here](https://github.com/micheleg/dash-to-dock/pull/1361/files).

On my machine, this meant editing the `~\.local/share/gnome-shell/extensions/dash-to-dock@micxgx.gmail.com/docking.js` file and editing the `if (animate) {` clause at line 1896 to read:

```
if (animate) {
  Main.overview.viewSelector._activePage = Main.overview.viewSelector._appsPage;
  Meta.later_add(Meta.LaterType.BEFORE_REDRAW, () => {
     grid.opacity = 255;
     grid.animateSpring(IconGrid.AnimationDirection.IN, this.mainDock.dash.showAppsButton);
  });
}
```

Until the fix is officially merged into the extension, this is a small workaround, but works perfectly.

## Credits

Photo by <a href="https://unsplash.com/@crrrrraig?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Craig McLachlan</a> on <a href="/s/photos/garden-gnome?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>
