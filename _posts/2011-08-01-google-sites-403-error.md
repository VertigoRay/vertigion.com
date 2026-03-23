---
layout: post
title: Google Sites - 403 Error
date: 2011-08-01 12:26
author: VertigoRay
comments: true
tags: [Uncategorized]
---
<p>Just added another customer to Google Apps and for some reason their Google Sites page displayed a Forbidden message.  I initially thought it was simply some issue with DNS resolution.  A couple days later when it persisted, I opted to do a little more digging (since DNS issues should self resolve after 24 hours ~max).  I noticed that when I browsed to their Sites admin page (<a href="https://sites.google.com/a/">https://sites.google.com/a/</a><em>domain</em>.com), there was a Forbidden message there as well (meaning it was never DNS related).<!-- more --></p>
<h2>The Fix</h2>
<div>Since I had <em><strong>just</strong></em> set-up Google Sites and <em><strong>only</strong></em> created a single site, without any configuration &hellip; I opted to <em><strong>uninstall </strong></em>Google Sites and re-install it.  The hope was that this would reset whatever settings got out of whack on the system.  </div>
<h2>The Result</h2>
<div>It worked!</div>
<h2>The Downside</h2>
<div>I lost my one created, but un-configured site.  This was not detrimental to me, but may be to you if you&rsquo;ve been using Google Sites for several years and have a lot of content created.</div>
<h2>The How</h2>
<div><ol><li>Login: mail.google.com with your Google Apps admin account.</li>
<li>Click the <em>Settings</em> tab.</li>
<li>Click <em>Sites</em> from the <em>Services</em> list.</li>
<li>Click <em>Uninstall Sites</em> under the <em>Uninstall service</em> section of the <em>General</em> tab.</li>
<li>Follow the onscreen prompts &hellip;</li>
</ol></div>
<div><em><strong>Note:</strong>  Please don&rsquo;t ask me how to get rid of the 403 error without uninstalling Google sites!  At this point, I don&rsquo;t know and have not had the desire/need to find out.</em></div>
