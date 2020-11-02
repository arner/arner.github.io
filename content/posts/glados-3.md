---
title: "Install the oucher"
date: 2020-11-01T22:05:18+01:00
draft: false
---

This post is part of a series.

- Do you want to [make GLaDOS clean your floor](/posts/#make-glados-clean-your-floor)?
- Have you [rooted your Xiaomi or Roborock vacuum cleaner](/posts/#root-your-xiaomi-vacuum)?
- Have you [installed the GLaDOS voice pack](/posts/#install-the-glados-voice-pack)?

Do you want to make her say stuff when she bumps into something?

Good, let's continue.

A vacuum robot that verbally reacts when it bumps into things is surprisingly funny.

{{< youtube AlV1m1Ropq8 >}}

It's also a good way to add a bit more personality to a robot that already has a [voice pack installed](/posts/#install-the-glados-voice-pack).

Thanks to a clever tool called [oucher](https://github.com/porech/roborock-oucher) it's easy to achieve it - without the hardware in the video. It's a program that runs on your vacuum (just like [Valetudo](https://valetudo.cloud/)). When it sees a 'Bumper' event in the logs, it plays a sound. You need a [rooted Xiaomi or Roborock vacuum cleaner](/posts/#root-your-xiaomi-vacuum) with SSH access.

## Installing the oucher

Clone the oucher repo and enter the directory. 

```bash
git clone https://github.com/porech/roborock-oucher.git
cd roborock-oucher
```

To make the next steps a bit easier, we'll assign the IP address of your vacuum to a variable. Find it on the status page of your router or with a mobile app called Fing.

```bash
export IP=192.168.xxx.x
```

First, SSH into the vacuum to install a few packages.

```bash
ssh root@$IP
```

From within the vacuum shell (it starts with `root@rockrobo:~#`):

```console
mkdir /mnt/data/oucher
apt-get -y update
apt-get -y install espeak sox alsa-utils
apt-get -y clean
exit
```

Back on your own computer, edit oucher.yml. Set `delay: 10` and`phrases: []` to make it not too chatty and to make it use sound files exclusively.

Now copy the files to the vacuum:

```bash
scp oucher.conf root@$IP:/etc/init/
scp oucher root@$IP:/usr/local/bin/
scp oucher.yml root@$IP:/mnt/data/oucher/
```

 [Download the sounds](http://s000.tinyupload.com/?file_id=81925662786830967138) and unzip them.

> It's more fun to create your own set.
> Find the right samples, for instance on [portal2sounds.com](http://www.portal2sounds.com/#w=glados)
> (download via 'direct link').
> Convert mp3 to wav like this: 
> ```bash
> for i in *.mp3; do ffmpeg -i "$i" "sounds/${i%.*}.wav"; done
> ```
> This requires [ffmpeg](https://ffmpeg.org/) to be installed.

Copy the sounds directory to the vacuum:

```bash
scp -r sounds root@IP:/mnt/data/oucher/
```

Make the program executable and start the service:

```bash
ssh root@$IP chmod +x /usr/local/bin/oucher
ssh root@$IP service oucher start
```

## Done!

Your vacuum should now sound a little something like this:

{{< youtube gldJOiC2ZSo >}}

Did you create your own voice pack? Share it in the [comments](https://www.youtube.com/watch?v=gldJOiC2ZSo) on YouTube!