---
title: "Root your Xiaomi vacuum"
date: 2020-11-01T19:45:24+01:00
draft: false
---

This guide describes how to root your Xiaomi/Roborock vacuum (meaning to gain administrator access). Check [here](https://builder.dontvacuum.me/) if it works on yours.

The standard firmware requires a connection to the internet and it transmits information about your wifi SSID, gateway mac address and other information like your floor plan to Xiaomi's servers every thirty minutes. If this makes you feel a bit uncomfortable, good, read on.

Rooting changes your vacuum cleaner from an appliance that needs the Chinese cloud to function to a computer that you own and control via your local wifi directly. A Xiaomi or Roborock vacuum cleaner is just a computer that runs linux. So as a bonus, you can do other cool stuff like installing custom voicepacks and other cool hacks. 

The world of vacuum rooting can be a bit overwhelming so here's a few names you might encounter:

- **DustCloud** is where it all started. It's a git repo by a guy called Dennis Giese that describes the process and tools for hacking Xiaomi devices.
- **DustBuilder** is a web tool by the same guy that can build the firmware for you so you don't have to do it yourself.
- **Valetudo** is is a standalone program which runs on rooted roborock vacuums and allows you to control it without the cloud.
- **Valetudo RE** is a fork of Valetudo with more functionality.
- **python-miio** is a tool to talk to Xiaomi devices.
- **Win Mirobot 1.1.0** is an alternative tool to install the firmware or voicepacks. We won't be using it in this guide.


This is what the interface of Valetudo looks like:

![valetudo screenshot](../valetudo.png)

Simple and practical.

# Getting started

We're gonna follow the following steps:

1. Download custom (rooted) firmware.
1. Get a secret token from your device.
1. Install the firmware on the vacuum (_flash_ it).
1. Set up Valetudo.

## Download custom firmware

### Generate SSH key

For our later steps, we will need an SSH key to be able to access the command line of the vacuum from the computer you're currently using.

You can let DustBuilder create one for you, but it's more secure to do it yourself. 

```bash
ssh-keygen -o -a 100 -t ed25519
```

### Build the custom firmware

Go to [https://builder.dontvacuum.me/](https://builder.dontvacuum.me/) and select your vacuum.

In the form, enter your email. Select _Your SSH-Public key_ and browse for it (it's `[your home directory]/.ssh/id_ed25519.pub`). The other default settings are fine.

After submitting, it can take a few minutes before you receive a link to your firmware in the mail. When you do, download it. It's called something like `v11_004018.pkg`. In the meantime, you can continue with the next step.

## Get the token from your device

Make sure you have the following software installed on your computer:

- python3
- python3-pip

Now execute the following commands:

```bash
pip3 install wheel
pip3 install python-miio
```

Reset the vacuum's wifi. For the first generation Xiaomi, this means pressing and holding the home and power buttons for 3-5 seconds.

After a short while your vacuum starts broadcasting a wifi access point. Move the device close to your computer and connect the computer to the access point. If you use a VPN, turn it off.

Run the following command to get your token:

```bash
mirobo --debug discover --handshake true
```

## Flash the firmware

Find your local IP address. In windows, run `ipconfig` and look for your IPv4 address. On MacOS or Linux, run `ifconfig | grep 192.`.

Before running the command below, make sure you replace the token value, your ip and the path to the built image.

```bash
mirobo --ip 192.168.8.1 --token <the token> update-firmware --ip <your ip address> <path/to/built/image.pkg>
```

It should look something like:

```bash
mirobo --ip 192.168.8.1 --token 0f33a5fd27d421ddb8980 update-firmware --ip 192.168.8.2 ../v11_004018.pkg
```

The update will take up to about 10 minutes. The vacuum will tell you when it's done.

## Connect to your vacuum 

After reconnecting your computer to the vacuums wifi, access the vacuum with:

```bash
ssh root@192.168.8.1
```

Because your public key was included in the firmware, you will be granted access. You're now a user in the linux operating system running on your vacuum cleaner! What a time to be alive.

Feel free to poke around a bit and take a moment to enjoy the hacker vibes. Think of the possibilities! You could run any linux program on here...

But first, restart the vacuum:

```bash
reboot -h now
```

### Set up Valetudo

When it's done rebooting (make sure your computer is connected to the vacuum's wifi again), visit `http://192.168.8.1` in your browser to set up the connection from the vacuum to your wifi router.

After doing that, the vacuum no longer has a wifi access point. Get its new IP address (for instance on your router's status page, or with a mobile app called Fing).

Visit the address in your browser. 

Done! You can now control your vacuum from your local network.

## Next steps

Now you have a rooted vacuum cleaner, let's [install a custom voice pack to it](/posts/#install-the-glados-voice-pack).
