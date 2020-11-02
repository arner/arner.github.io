---
title: "Install the GLaDOS voice pack"
date: 2020-11-01T21:42:20+01:00
draft: false
---

This post is part of a series.

- Do you want to [make GLaDOS clean your floor](/posts/#make-glados-clean-your-floor)?
- Have you [rooted your Xiaomi or Roborock vacuum cleaner](/posts/#root-your-xiami-vacuum)?

Good. Let's continue.

The lazy version of this is actually really simple.

# The lazy version

1. Download the GLaDOS voice pack [here](https://github.com/arner/roborock-glados/releases/download/v1/custom-v1.zip) and unzip it.
2. Visit Valetudo in your browser (the IP of your vacuum) and go to *Settings -> Sound and voice*. Upload the downloaded .pkg file and press *Upload Voice Pack*.
3. Done!

> Alternatively, you can use [python-miio](https://python-miio.readthedocs.io/en/latest/vacuum.html#sounds), the tool we installed in the [previous step](/posts/#root-your-xiaomi-vacuum).

# The creative version

The voice pack above consists of two parts; generated text-to-speech and actual Portal samples.

You can customize both of them, meaning that you can make GLaDOS say whatever you want her to! [The README of my git repo](https://github.com/arner/roborock-glados) describes how to do it. On a high level, you'll:

1. Clone the repo and install a few tools
2. Customize the text of what the text-to-speech engine will say
3. Download and convert original samples of your choice
4. Package the sounds.

After this, you can go back to step 2 of the lazy version.

# Next step

The Xiaomi vacuum is too quiet and polite for GLaDOS, we need a bit more character. Luckily, there's the aptly named tool _oucher_ that can make her talk whenever she bumps into something.

Lets [install the oucher](/posts/#install-the-oucher)!