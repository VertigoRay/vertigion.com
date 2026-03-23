---
layout: post
title: Active Directory - Validate Group SamAccountName
date: 2012-02-21 21:34
author: VertigoRay
comments: true
tags: [Active Directory, AD, posh, powershell, RegEx, Regular Expression, Uncategorized]
---
<p>Did a lot of digging to find the details needed so that I can write a Reg Ex to validate an AD Group SamAccountName.  Here&rsquo;s what I got and I wanted to share &hellip;<!-- more --></p>
<pre>(^[^. ""/\[]:|\+=;?*][^""/\[]:|\+=;?*]{1,63})(?(1)|[^.]$)</pre>
<p>This translates to:</p>
<ul><li>Not Starting with a period (.) or a space ( ).
<ul><li>This, by design, excludes strings consisting solely of all periods (.) or spaces ( ).</li>
</ul></li>
<li>Between 1 and 64 characters (inclusive) long.</li>
<li>Not including any of these characters:
<ul><li>&ldquo;/[]:|&lt;&gt;+=;?*</li>
</ul></li>
<li>Not ending in a period (.).</li>
</ul><p>If you&rsquo;re like me, you want justification:</p>
<ul><li><a href="http://support.microsoft.com/kb/938447">http://support.microsoft.com/kb/938447</a></li>
<li><a href="http://technet.microsoft.com/library/Cc975532">http://technet.microsoft.com/library/Cc975532</a></li>
<li><a href="http://technet.microsoft.com/en-us/library/cc776019.aspx">http://technet.microsoft.com/en-us/library/cc776019.aspx</a></li>
<li><a href="http://support.microsoft.com/kb/909264">http://support.microsoft.com/kb/909264</a></li>
<li><a href="http://forums.techarena.in/active-directory/1011758.htm">http://forums.techarena.in/active-directory/1011758.htm</a></li>
</ul><p>I realize the page says 63 characters, but my testing shows 64 to be valid (Win2k8).</p>
