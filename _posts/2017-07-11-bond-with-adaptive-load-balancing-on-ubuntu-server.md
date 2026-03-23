---
layout: post
title: Bond with Adaptive Load Balancing on Ubuntu Server
date: 2017-07-11 06:13
author: VertigoRay
comments: true
tags: [Linux]
---
I was wanting to setup my server with bonded network interface cards (NICs). Since it took me a while to get the bond working, I thought I do some notes to self here; added benefit of sharing with you all.

# Configuration

This is my `/etc/network/interfaces` file:

```bash
# This file describes the network interfaces available on your system
# and how to activate them. For more information, see interfaces(5).

source /etc/network/interfaces.d/*

# The loopback network interface
auto lo
iface lo inet loopback

# The primary network interface
allow-hotplug enp11s0f0
  iface enp11s0f0 inet manual
  bond-master bond0

allow-hotplug enp11s0f1
  iface enp11s0f1 inet manual
  pre-up sleep 5 # Come up 5 seconds after enp11s0f0
  bond-master bond0

allow-hotplug enp3s0
  iface enp3s0 inet manual
  pre-up sleep 5 # Come up 5 seconds after enp11s0f0
  bond-master bond0

allow-hotplug enp2s0
  iface enp2s0 inet manual
  pre-up sleep 5 # Come up 5 seconds after enp11s0f0
  bond-master bond0

# The bonded interface
allow-hotplug bond0
  iface bond0 inet dhcp
  bond-primary enp11s0f0
  bond-slaves enp11s0f0 enp11s0f1 enp3s0 enp2s0
  bond-mode 6 # balance-alb
  bond-miimon 100
  bond-downdelay 200
  bond-updelay 200
  bond-lacp-rate 1
```

You'll notice that my NICs aren't `eth0`, `eth1`, `eth2`, and `eth3`. Nope, that would be way too easy. In order to get the names of my NICs, I used: `ip link show`.

# Decisions

<!-- <img class="size-medium wp-image-282 alignright" style="margin-top: 0.857143rem; margin-bottom: 0.857143rem; margin-left: 1.71429rem;" src="https://vertigion.com/wp-content/uploads/2017/07/adaptive-load-balancing1-300x279.png" alt="Adaptive Load Balancing" width="300" height="279" /> -->
![Adaptive Load Balancing](https://vertigion.com/wp-content/uploads/2017/07/adaptive-load-balancing1-300x279.png)

I chose to use `allow-hotplug` on the NICs instead of `auto`. While booting with `auto` set, I was getting a significant delay: *a start job is running for raise network interfaces (5 mins 1 sec)*. This is because `auto` causes the system to wait for DHCP relay, even if it's set to `manual`.

Additionally, I chose to have my main NIC boot instantly while the other three NICs would wait five seconds (`pre-up sleep 5`). This is one method of ensuring that my main NIC was the one that the bond spoofed the mac address of. Another method would have been to specify the `hwaddress` in the bond; such as:

```bash
auto bond0
iface bond0 inet manual
hwaddress fe:80:12:04:6d:6f
```

I hope this helps others out there in the ether!
