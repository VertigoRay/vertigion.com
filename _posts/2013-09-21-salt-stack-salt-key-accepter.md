---
layout: post
title: Salt Stack - salt-key-accepter
date: 2013-09-21 19:51
author: VertigoRay
comments: true
tags: [Salt Stack, Uncategorized]
---
During my <a href="http://go.vertigion.com/AdventuresWithSaltStack">adventures with Salt Stack</a>, I wanted to accept keys automatically on the master. However, I wanted to do this more intelligently than just accepting all of them.

To solve this, I threw together a quick script that would accept keys of salt-minions that were in an approved subnet. I use <code>incrontab</code> to watch the <code>minions_pre</code> directory for new keys to be created. It then fires off the <code>salt-key-accepter</code> script.

Here is the logical flow of the script:

<ul>
<li>Accept Key temporarily.</li>
<li>Tell the minion to Sync its grains.</li>
<li>Pull the IPv4 info from grains.</li>
<li>Check if any of the IPs are in the allowed <code>allowed_ip_cidrs</code> list.</li>
<li>If Allowed: trigger <code>state.highstate</code>.</li>
<li>If Denied: move key to rejected list.</li>
</ul>

To get the key off of the rejected list and into the approved list, you need to use the bash <code>mv</code> command, because <a href="https://github.com/saltstack/salt/issues/7399">salt-key won't move keys between accepted and rejected</a>. Keys are stored in the <a href="http://docs.saltstack.com/ref/configuration/minion.html#pki-dir">pki_dir</a>.

If you want to know how to use it, <a href="https://github.com/VertigoRay/salt-key-accepter/blob/master/README.md">check out the readme</a>.

If you have issues with it, please <a href="https://github.com/VertigoRay/salt-key-accepter/issues">use the issue tracker</a>.

General questions and comments can be made here and I'll get to them as I can.

Tested on: Debian Squeeze
