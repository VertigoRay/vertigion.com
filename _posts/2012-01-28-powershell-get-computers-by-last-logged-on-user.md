---
layout: post
title: PowerShell - Get Computers by Last Logged on User via SCCM
date: 2012-01-28 18:05
author: VertigoRay
comments: true
tags: [Function, posh, powershell, scripting, Uncategorized]
---
<p>I needed a versatile script to query SCCM and return the list of computers associated with that user.  This is my solution to that.  It simply queries SCCM for the list of computer’s whose last logged on user matches the supplied SamAccountName.  I&rsquo;ve populated the PowerShell Comment Doc, so check out the Examples supplied in there for usage.<!-- more --></p>
<div class="gist"><a href="https://gist.github.com/VertigoRay/6343964">https://gist.github.com/VertigoRay/6343964</a></div>
<p>Hope it helps others out there.  Just add the function to your PowerShell Profile or as a Functions in a script to make it available!</p>
<p>In case you&rsquo;re wondering what the reference is to the <em>$_ProgressTotal</em> variable &hellip; I that&rsquo;s a Global Variable that I set when piping large sets into this funtion.  I&rsquo;ve got an example of it being used in my <a href="http://go.vertigion.com/PowerShell-Get-ActiveCompByDept" title="PowerShell: Get-ActiveCompByDept" target="_self">Get-ActiveCompByDept</a> function.</p>
<p>Thanks to Jeffrey B. Murphy (sourced) for the starting point.</p>
