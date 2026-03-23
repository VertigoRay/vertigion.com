---
layout: post
title: Adventures with Salt Stack
date: 2013-08-20 19:15
author: VertigoRay
comments: true
tags: [Active Directory, Apple, GPO, OSX, Salt Stack, Uncategorized]
---
<p>I have been in IT for over 15 years. most of that tenure has been managing Windows - mostly in the Desktop/Laptop realm. This means that I’ve got a lot of experience with AD, GPO, SCCM, scripting, etc.</p>
<p>Recently, I took over the management of Apple Desktops/Laptops for the organization that I work for. Before now, Apples were the outlier — just give the user admin rights and good luck. Now that it’s my purview, I’m opting for more centralized management technique. Something to, as closely as possible, mimic GPO (for managing settings) and SCCM (for managing software) for Windows.<!-- more --></p>
<p>This is where <a href="http://saltstack.org" target="_blank">Salt Stack</a> and <a href="https://code.google.com/p/munki" target="_blank">Munki</a> come into the picture. Those names kind of sound like a couple of lame superheroes, right? /snicker</p>
<p><em><strong>Note:</strong>  I don&rsquo;t have a lot of experience with Munki (yet!) because I&rsquo;ve got one of my people working on it.  That&rsquo;s right, I&rsquo;ve got people!</em></p>
<h1>Salt Stack</h1>
<p><a href="http://docs.saltstack.com/#what-is-salt-stack" target="_blank">What is Salt Stack?</a></p>
<p>Honestly, <a href="http://saltstack.org" target="_blank">SaltStack</a> wasn&rsquo;t my first pick.  I tried out <a href="http://puppetlabs.com" target="_blank">Puppet</a> and it worked great!  So why did I switch?  Puppet is not a complete product for what I want to do, and I want to contribute and help fill the gaps.  Frankly, the shortcoming was that I don&rsquo;t know <a href="https://www.ruby-lang.org/">Ruby</a> and have no desire to learn Ruby.  Nothing against Ruby &hellip; I just didn&rsquo;t want to learn yet another language to accomplish this task.  Especially, if there is something written in a language I know.</p>
<p>I quickly turned my head towards Salt Stack, written in <a href="http://python.org/">Python</a>.  In an afternoon, I had a Salt Master up on Debian and Apple OS X.8.4 connecting to it and ready for commands.  I&rsquo;ve since written a couple of modules/grains/scripts for Salt Stack to help me accomplish what I&rsquo;m doing.  You can keep up with the running list of <a href="http://github.com/VertigoRay" target="_blank">my</a> Salt Stack programming projects <a href="http://blog.vertigion.com/tagged/Salt-Stack">my Salt Stack tag filter</a>.</p>
