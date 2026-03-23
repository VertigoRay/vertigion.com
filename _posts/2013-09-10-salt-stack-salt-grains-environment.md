---
layout: post
title: Salt Stack - salt-grains-environment
date: 2013-09-10 21:53
author: VertigoRay
comments: true
tags: [Apple, Salt Stack, Uncategorized]
---
<p>During my <a href="http://go.vertigion.com/AdventuresWithSaltStack">adventures with Salt Stack</a>, I was having issues wrapping my head around how I was going to control what environment my dev machines were in.  I even thought that pillar[environment] returning None <a href="https://github.com/saltstack/salt/issues/7088" target="_blank">was a bug</a>.  Turns out, I just needed to think more about what process would work in my environment.<!-- more --></p>
<p>To solve this, I have created a very simple custom grain file that would store and set the our `dev` and `pre` environments.</p>
<p>If you want to know how to use it, <a href="https://github.com/VertigoRay/salt-grains-environment/blob/master/README.md" target="_blank">check out the readme</a>.</p>
<p>If you have issues with it, please <a href="https://github.com/VertigoRay/salt-grains-environment/issues" target="_blank">use the issue tracker</a>.</p>
<p>General questions and comments can be made here and I&rsquo;ll get to them as I can.</p>
<p>Tested on: 10.8.4</p>
