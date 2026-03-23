---
layout: post
title: Synergy - Send CTRL+ALT+DEL to Client
date: 2014-01-02 21:19
author: VertigoRay
comments: true
tags: [Uncategorized]
---
I'm mostly posting this here for ease of finding it in the future.
It seems like most people prefer to disable CTRL+ALT+DEL instead of actually allowing the *Synergy service* to initiate the Secure Attention Sequence (aka: CTRL+ALT+DEL).

Since the location of the Policy setting changes from Win7 to Win8, I'm going to tell you how to search/filter for it.
**From the client**, follow these steps:

- Lauch *Local Group Policy Editor<br />*(Run: *gpedit.msc*)
- Browse to: **Computer Configuration &gt; Administrative Templates &gt; All Settings**
- Click *Action &gt; Filter Options*, from the Main Menu.
- Check the box to *Enable Keyword Filters*.
- Filter for the word:  **Ease**
- Click: OK

The only (as of the time of this writing) policy that should show up is **Disable or enable software *Secure Attention Sequence***.
Edit the policy and ensure the settings are:

1. **Enabled**
1. Options: **Services
   <br />***(Note:  **Services and Ease of Access applications** will also work.)*

Once you click **Apply**, the settings will take affect.
Press **CTRL+ALT+PAUSE/BREAK** to initiate the Secure Attention Sequence on the client.

