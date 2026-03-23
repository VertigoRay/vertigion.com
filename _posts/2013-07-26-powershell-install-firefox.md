---
layout: post
title: PowerShell - Install Firefox
date: 2013-07-26 20:16
author: VertigoRay
comments: true
tags: [Uncategorized]
---
<p>I <a href="http://go.vertigion.com/MozillaMaintenanceService">previously posted</a> about tweaking the Firefox install to prevent the <em>Mozilla Maintenance Service</em> from installing.  <a href="http://go.vertigion.com/MozillaMaintenanceService#comment-975826718">Ph0neutria asked an off topic</a> that intrigued me, so this is the answer to that.</p>
<p>My solution installs a particular version and language of Firefox.  If the language is not specified, it will check for a current install and match that language, defaulting to en-US.<!-- more --></p>
<div class="gist"><a href="https://gist.github.com/VertigoRay/6091281">https://gist.github.com/VertigoRay/6091281</a></div>
<div><em><strong>Note:</strong> You&rsquo;ll want to sign the code so you can run it without changing the execution policy to unrestricted.</em></div>
<div></div>
<div>Sample Usage:</div>
<pre>InstallFirefox.ps1 22.0</pre>
<pre>InstallFirefox.ps1 22.0 'es-ES'</pre>
<p>I know the code isn&rsquo;t commented, but I just use the `Write-Debug` lines as comments.  To see the debug output, you&rsquo;ll need to set the DebugPreference variable:</p>
<pre>$DebugPreference='Continue'</pre>
<p>Thought about, allowing <em>&ldquo;latest&rdquo;</em> for the version, but that would be a bit more complex since you would have to parse the page to get the version number from the latest version.  Not dificult, but beyond the scope of this solution.</p>
