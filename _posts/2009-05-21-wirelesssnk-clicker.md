---
layout: post
title: WirelessSNK Clicker
author: VertigoRay
img: //i.imgur.com/CzRX0Dz.png
comments: true
tags:
- AutoIt
- WiFi
---
The WirelessSNK Clicker is my solution to a deployment issue.
I needed to automate the configuration of a wireless network.

I used the *Wireless Network Setup Wizard* in Windows XP to create my setupSNK.exe.
This process was extremely easy … I’ll give Microsoft props on this one.
For those that do not know, you can find the Wizard in the *Control Panel*.

My dilemma was that setupSNK.exe does not accept any of the standard switches to make it run silently …

- `/s`
- `/silent`
- `/install`
- `/q`
- `/quiet`
- `/qb`
- `/?`

Everything I tried only gave me the same pop-up window that I got from simply running the executable.

After doing some googling I found several options.
Some of the alternatives that I found suggested things like installing Daemon Tools, making an image of the folder, and auto-running it from a mounted image.
They even suggested using AutoIt to automate the install of DaemonTools.
This was a lot more work than I wanted to do, and since I’m pretty proficient with AutoIt, I decided to do something a little better.

# My Solution

I wrote an AutoIt script to click OK for me in the two windows that appear.
Much easier than mounting it and all of that jazz.
So, here’s meat and potatoes of the script:

```autoit
Local $setupSNK = @ScriptDir & "setupSNK.exe"
Run($setupSNK)
WinWaitActive("Wireless Network Setup Wizard", "Do you want to add this computer to the wireless network")
Send("{ENTER}")
WinWaitActive("Wireless Network Setup Wizard", "You have successfully added this computer to the wireless network")
Send("{ENTER}")
```

That is really all you need to compile to make it work.
I, being the helpful fella that I am, wrote it to be a bit more versatile so that I could share it.

Here’s my help message, which should give you the complete overview on its functionality:

![WirelessSNK_Clicker_help](//i.imgur.com/J1uKQ.jpg)

If you want to compile that yourself, feel free! You can start your AutoIt success stories by visiting [AutoItScript.com](http://www.autoitscript.com/).

Otherwise, feel free to download my pre-compiled version for free ([7z compression](http://lmgtfy.com/?q=7z+compression "7z compression"))!

- [Download WirelessSNK Clicker (.7Z) \[233K\]](http://static.vertigion.com/files/WirelessSNK_Clicker-v0.1.7z "WirelessSNK_Clicker-v0.1.7z")
- [Download WirelessSNK Clicker with Source (.7Z) \[234K\]](http://static.vertigion.com/files/WirelessSNK_Clicker_wSrc-v0.1.7z "WirelessSNK_Clicker_wSrc-v0.1")

- - -

RapidShare:

- [Download WirelessSNK Clicker (.ZIP) \[238K\]](http://rapidshare.com/files/235697392/WirelessSNK_Clicker.zip)
- [Download WirelessSNK Clicker with Source (.ZIP) \[240K\]](http://rapidshare.com/files/235697393/WirelessSNK_Clicker_wSrc.zip)