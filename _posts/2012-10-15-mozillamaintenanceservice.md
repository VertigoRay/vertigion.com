---
layout: post
title: Mozilla Maintenance Service
date: 2012-10-15 19:42
author: VertigoRay
comments: true
tags: [deployment, mozilla, Uncategorized]
---
<p>So &hellip; I finally got around to removing the <em>Mozilla Maintenance Service</em> from my deployment of <em>Firefox</em>.  It was pretty easy, but surprisingly unexplored by the blogging community.  I&rsquo;m by no means calling first on this post, just saying I did it through self-discovery.<!-- more --></p>
<p><strong>Why would I want to remove this service?</strong>  Quite simply, we have an automated process for our current roll out of <em>Firefox</em> updates.  <em>Mozilla Maintenance Service</em> is a long overdue, but unnecessary feature for my infrastructure.  Additionally, I don&rsquo;t like things being installed arbitrarily.  <em>I may investigate if I would rather use this service for silent updates of Firefox in the future.</em></p>
<p>I can boil this process down to two basic needs:</p>
<ul><li><strong>Block Service Install<br /></strong>Prevention of future and new installs of the <em>Mozilla Maintenance Service.</em></li>
<li><em><span><strong>Silent Removal</strong><br />Silently removal of the </span><em>Mozilla Maintenance Service</em><span> from systems with it already installed.</span></em></li>
</ul><h1>Block Service Install</h1>
<div>
<div><em><strong>Note:</strong> Thanks to Ph0neutria for helping me  determine that this doesn&rsquo;t work for new installs.  It&rsquo;s just for the upgrade, as the bugzilla report states.  See the comments for details.</em></div>
<h2>Fresh Install</h2>
<div>Create an .ini file to disable the install of the <em>Mozilla Maintenance Service</em>, as shown:</div>
<pre>[Install]
MaintenanceService=false</pre>
<div>Install firefox with the /INI switch, as shown (assumes the .ini file is in the same directory as the Firefox setup.exe):</div>
<pre>Firefox Setup 22.0.exe /INI="C:FullPathtoFileFirefox.ini"</pre>
<div><a href="http://mzl.la/T5FuNp" title="Installer:Command Line Arguments - MozillaWiki" target="_blank">Supporting Documentation.</a></div>
<h2>Application Update</h2>
<div>Block <em>Mozilla Maintenance Service</em> from installing by running the following command BEFORE you run the <em>Firefox Setup X.X.X.exe</em> installer:</div>
<div>
<ul><li>REG ADD HKLMSOFTWAREMozillaMaintenanceService /v Attempted /t REG_DWORD /d 1 /f</li>
</ul></div>
<div><a href="http://mzl.la/T5FuNp" title="Windows Service Silent Update - Service installation" target="_blank">Supporting Documentation.</a></div>
</div>
<h1>Silent Removal</h1>
<div><span>You can silently uninstall the <em>Mozilla Maintenance Service</em> with the following command:</span></div>
<div>
<ul><li><span><strong>x64 Systems:</strong><br />&ldquo;%ProgramFiles(x86)%Mozilla Maintenance ServiceUninstall.exe&rdquo; /S /v&quot;qn&quot;</span></li>
<li><span><strong>x86 Systems:</strong><br />&ldquo;%ProgramFiles%Mozilla Maintenance ServiceUninstall.exe&rdquo; /S /v&quot;qn&quot;</span></li>
</ul><span></span></div>
<div></div>
