---
layout: post
title: Salt Stack - salt-osx-grains
date: 2013-08-22 21:40
author: VertigoRay
comments: true
tags:
- Apple, 
- SaltStack
redirect_from: /post/60873973706/salt-stack-salt-osx-grains
---
During my [adventures with Salt Stack](http://go.vertigion.com/AdventuresWithSaltStack), I found that the [grains](http://docs.saltstack.com/topics/targeting/grains.html) lacked a couple of key bits on information for OS X:

- [Apple Model Number](http://bit.ly/189M2So)
- [Apple Serial Number](http://bit.ly/15Pz902)

Additionally, I decided that it would be useful to store the computer's *Company Asset Number* in [nvram](http://bit.ly/17OBJRL), and make that available in grains.<!-- more -->

To solve this, I made some customized grains and [made them available on GitHub](https://github.com/VertigoRay/salt-osx-grains).

If you want to know how to use the customized grains, [check out the wiki](https://github.com/VertigoRay/salt-osx-grains/wiki).

If you have issues with the custom grains, please [use the issue tracker](https://github.com/VertigoRay/salt-osx-grains/issues).

General questions and comments can be made here and I’ll get to them as I can.

Tested on: 10.8.4
