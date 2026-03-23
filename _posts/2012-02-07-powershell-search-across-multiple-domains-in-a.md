---
layout: post
title: PowerShell - Search Across Multiple Domains in a Forest
date: 2012-02-07 20:46
author: VertigoRay
comments: true
tags: [Active Directory, AD, Global Catalog, LDAP, posh, powershell, Uncategorized]
---
<p>While trying to find a quick answer to search across domains for duplicated user accounts, I came across a blog (sourced) that pointed me in a good direction.  To skip the meat of it, we have a Global Catalog setup on our AD and I found it more useful to target that and search the entire forest, then to attach to each domain individually.  I hope this helps others out there in a similar scenario&hellip;<!-- more --></p>
<div class="gist"><a href="https://gist.github.com/VertigoRay/6357248">https://gist.github.com/VertigoRay/6357248</a></div>
<p>The above code will return you all Domain <em>User</em> Accounts in the Forest with the <em>SamAccountName</em> of <em>user0123</em>.  If you&rsquo;re like me, you&rsquo;ll want to handle the results from <em>FindAll()</em> instead of just dumping it to the pipeline:</p>
<div class="gist"><a href="https://gist.github.com/VertigoRay/6357306">https://gist.github.com/VertigoRay/6357306</a></div>
<p>As you can see, I&rsquo;ve moved the <em>FindAll()</em> method to the <em>foreach</em> conditional for handling. Of course, I&rsquo;m still dumping the results to pipline, but it&rsquo;s a nice, clean object that is pre-formatted just the way I want it.</p>
<p>I hope this helps others out there!</p>
