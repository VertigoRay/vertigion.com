---
layout: post
title: Win7 Handy RunAs Shortcut
date: 2010-07-01 21:00
author: VertigoRay
comments: true
tags: [Uncategorized, Win7]
---
<p>Maybe I’m just late in the game on this one, but I thought I would share this incredibly useful Win7 shortcut when running things as a different user. This really only applies when you&rsquo;re on a Win7 computer bound to a domain, but read on and enjoy &hellip;<!-- more --><br /><br /> When running something as a different user in Win7, you are prompted, as shown:</p>
<div>
<div><img border="0" src="https://sites.google.com/a/vertigion.com/archives/blog/win7-handy-runas-shortcut/RunAs_Empty%5B1%5D.png" /></div>
<br /> Well, normally if I wanted to run something as my local Admin account, I would have to specify my Computer Name, as shown:<br /><div><img border="0" src="https://sites.google.com/a/vertigion.com/archives/blog/win7-handy-runas-shortcut/RunAs_MyComputer%5B1%5D.png" /></div>
<br /> (Notice, this causes the Domain to automatically change.)<br /><br /> This is fine and dandy, but I was messing around last night and was a little annoyed cause I couldn&rsquo;t remember my Computer Name off the top of my head. I actually was just goofing around and discovered that Win7 understands dot (.) syntax, as shown:<br /><div><img border="0" src="https://sites.google.com/a/vertigion.com/archives/blog/win7-handy-runas-shortcut/RunAs_Dot%5B1%5D.png" /></div>
<br /><br /><div>Maybe all the VB scripting that I&rsquo;ve been doing lately played a part in this discovery. Either way, it’s going to save me a lot of time in the future! I hope you all find it equally as useful!<br /><br /> For those who think that just typing Administrator will do the trick&hellip;<br /><br /> Not when the Domain is specified (because you’re logged into a domain user) like in the previous images. If you do that, it’ll go for DomainAdministrator, possibly resulting in the following response:<br /><div><img border="0" src="https://sites.google.com/a/vertigion.com/archives/blog/win7-handy-runas-shortcut/RunAs_Error%5B1%5D.png" /></div>
<br /><br /> Note: I did check in XP SP3 and it does NOT work. I have not tried Vista. Also, Computer Names and Domains in the screenshots have been changed to protect the innocent.<br /><br /><strong>Demonstrated on:  Win7 Enterprise 64 bit</strong></div>
</div>
