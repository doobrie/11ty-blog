---
title: Quick Tip No 1 - SynthWave '84 Not Glowing ?
date: 2021-03-29
tags: [tip, vscode]
---

![](/images/quick.jpg)

I use [SynthWave '84](https://github.com/robb0wen/synthwave-vscode) as my theme in VS Code and love the "gratuitous 80s glow" that is provided when Neon Dreams is enabled.

![Screenshot of Visual Studio Code With SynthWave Theme](https://cdn.hashnode.com/res/hashnode/image/upload/v1617051416172/3nB07Bzhp.png)

Unfortunately, the glowing aspect of the theme doesn't work out of the box when you've got VS Code installed in a shared directory - such as when you've installed it via a `.rpm` or `.deb` package.

On my Fedora system, VS Code is installed into `/usr/share/code`. To enable Neon Dreams, install and select the SynthWave theme from VS Code plugins.

Next, we need to ensure that VS Code is writable to the current user. This can easily be achieved by executing:

```bash
sudo chown -R "$(whoami)" /usr/share/code
```

We can now enable Neon Dreams by selecting the Control Bar in VS Code (Ctrl+Shift+P) and selecting `SynthWave '84: Enable Neon Dreams`. One reboot later, and your back in the 1980's.

You can then set the permissions back on the VS Code directory by executing:

```bash
sudo chown -R root /usr/share/code
```

## Credits

Photo by <a href="https://unsplash.com/@the_gerbs1?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Jean Gerber</a> on <a href="https://unsplash.com/s/photos/quick?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>
