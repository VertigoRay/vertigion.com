---
layout: post
title: Google Chrome - Block Flash (& all plug-ins) without an Extension
date: 2012-01-25 03:52
author: VertigoRay
comments: true
tags: [Chrome, Flash, Java, Plug-ins, QuickTime, Silverlight, Uncategorized]
---
<p>I&rsquo;ve been using <a href="http://www.google.com/chrome" title="Google Chrome" target="_blank">Google Chrome</a> instead of <a href="http://firefox.com" title="Firefox" target="_blank">Firefox</a> for quite a while.  I moved mostly for the simpler functionality and aesthetics.  When I first jumped ship there were not a lot of extensions available for <a href="http://www.google.com/chrome" title="Google Chrome" target="_blank">Google Chrome</a>, but that quickly became a non-issue as many extension developers ported their stuff over.</p>
<p>Getting to the point, today I was leaning over a co-workers shoulder and saw his flash blocker icon in on his toolbar and it reminded me that I used to use the extension in <a href="http://firefox.com" title="Firefox" target="_blank">Firefox</a>, but never re-checked for it after my initial set-up.  As I was trying to determine which of the two top-rated <a href="http://chrome.google.com/webstore/search/flash%20block" title="Flash Block Extensions" target="_blank">Flash Block extensions</a> I wanted to use, I read <a href="http://bit.ly/wBRva7" title="Chrome Review" target="_blank">a review (dated: May 5, 2011)</a> stating that <a href="http://www.google.com/chrome" title="Google Chrome" target="_blank">Google Chrome</a> will block flash natively.</p>
<p><em><strong>Hark!</strong></em></p>
<p>So I set it up, tested it out, and love its simple yet effective functionality.  Here are the details.<!-- more --></p>
<h1>Setup</h1>
<p><em>Note:  These instructions disable ALL plugins, not just Flash.  This was a perk for me.  Luckily, they&rsquo;re all easily managed &hellip; just check out the Usage section below.</em></p>
<p><em>Note:  These instructions are subject to change.  If you have issues following them, check out <a href="https://support.google.com/chrome/bin/answer.py?answer=142064" title="Google Answers - Plug-ins" target="_blank">Google Answers</a> or try <a href="http://lmgtfy.com/?q=google+chrome+block+plugins" title="Google Chrome Block Plugins" target="_blank">Googling for a solution</a>.</em></p>
<p><strong>NOTE:  If you&rsquo;re currently in Chrome, you can simply point your browser to the following URL and jump to step 5:  <a href="//settings/content">chrome://settings/content</a></strong></p>
<ol><li>Click the wrench icon <img align="middle" alt="Chrome Tools Menu" height="29" src="http://i.imgur.com/S0xG0.gif" width="29" /> to the right of your address bar.</li>
<li>Click <em>Options</em> (<em>Preferences </em>on Mac and Linux; <em>Settings</em> on Chrome OS), as shown:<br /><img alt="Chrome-BuiltInFlashBlock01" height="474" src="http://i.imgur.com/iahHp.png" width="319" /> </li>
<li>Click <em>Under the Hood</em>, to edit that section.</li>
<li>In the <em>Privacy</em> Section, Click the <em>Content settings</em> button, as shown:<br /><img alt="Chrome-BuiltInFlashBlock02" height="224" src="http://i.imgur.com/xRNtX.png" width="500" /> </li>
<li>Scroll down to the <em>Plug-ins</em> section.</li>
<li>Select <em>Block all</em>, as shown:<br /><img alt="Chrome-BuiltInFlashBlock03" height="112" src="http://i.imgur.com/FOzf2.png" width="376" /> </li>
</ol><p>Your settings should auto-save.  Don&rsquo;t worry about customizing permissions for specific websites; I&rsquo;ll explain why below.</p>
<h1>Usage</h1>
<p><em>Note:  The Setup instructions above disables ALL plugins, not just Flash.  This was a perk for me.  Luckily, they&rsquo;re all easily managed &hellip;</em></p>
<p>Now to show you how it worked for me (<em>version 16.0.912.75 m</em>).  First, I browsed over to <a href="http://youtube.com/VertigoRay" title="YouTube" target="_blank">YouTube</a> and picked a random video.  Voila! Instead of a video, I got a gray box, as shown:<br /><img alt="Chrome-BuiltInFlashBlock04" src="http://i.imgur.com/bnFEa.png" width="100%" /> </p>
<p>Running that video is a breeze, I just right click the gray box and select <em>Run this plugin</em> from the context menu, as shown (<em>Hmmm &hellip; plug-in or plugin? Hyphenation is the question!</em>):<br /><img alt="Chrome-BuiltInFlashBlock05" src="http://i.imgur.com/9Wrlg.png" width="100%" /> </p>
<p>What&rsquo;s more, I got an icon at the end of the address bar that shows you that the page you&rsquo;re on has blocked plug-ins, as shown:<br /><img alt="Chrome-BuiltInFlashBlock06" height="65" src="http://i.imgur.com/D5D3Y.png" width="209" /> </p>
<p>Clicking that icon gives you some useful configuration options, as shown:<br /><img alt="Chrome-BuiltInFlashBlock07" height="201" src="http://i.imgur.com/09cgD.png" width="293" /></p>
<h1>Conclusion</h1>
<p>I hope this helps!  There&rsquo;s no sense installing extensions to do what the browser can already handle quite well on its own.  What&rsquo;s more, I don&rsquo;t have to install multiple extensions to disable multiple plug-ins, since this process disables all plug-ins, not just Flash.  Simpler can definitely be better!</p>
<p>For a complete list of all your plug-ins, browse over to <a href="//plugins/">chrome://plugins</a>.</p>
