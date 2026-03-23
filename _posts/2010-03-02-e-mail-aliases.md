---
layout: post
title: E-mail Aliases
date: 2010-03-02 16:45
author: VertigoRay
img: //i.imgur.com/tS01GIT.jpg
comments: true
tags:
- E-mail
- Spam
---
For a while, I've been creating an email alias for every site that I have to create an account for.

 For example, let's say that my email address is `example@vertigion.com`
  When I went to [DreamHost.com](https://www.dreamhost.com/donate.cgi?id=9535) and made an account for them, I would create an alias such as:

*`example.dreamhost.com@vertigion.com` > `example@vertigion.com`*

Email that was sent to `example.dreamhost.com@vertigion.com` would show up in my `example@vertigion.com` inbox.<!-- more -->

# Why?

If [DreamHost.com](https://www.dreamhost.com/donate.cgi?id=9535) ever sells my email, I know exactly who is selling my email address.
When they do sell it, I can redirect all mail to that address to my junk account instead of blocking each spammer as my email is resold.
Basically, I could set a rule to block the To address instead of every new From address.

Anyway, it was easy enough to do for me cause I know how to do it, but then I told some of my family about how I prevent spam mail and they got excited and wanted to do it too.
Of course, I wasn't comfortable giving them admin access to my server so they could alias their email addresses, so I did some digging in the interwebs and found some interesting information referencing [sub-addressing](http://en.wikipedia.org/wiki/E-mail_address#Sub-addressing).

Wow! Now, I don't even have to log into the server to alias my address for each website! It's like auto-aliasing:

*example+dreamhost.com@vertigion.com > `example@vertigion.com`*

# How do you know if my email server supports sub-addressing?

Try it!!
`Example+test@vertigion.com` would be a good test.
Apparently, some [sub-addressing](http://en.wikipedia.org/wiki/E-mail_address#Sub-addressing) support hyphens as well, so you might want to try `example-test@vertigion.com`.
If either (or both) show up in your inbox, you'll know it works! 
How cool is that?

The best part is, I don't have to subject my email server to people who don't really know what they are doing.

# Any downsides?

- Well &hellip; it is a supported/used syntax, so it's possible that a hacker/email-seller/email-buyer could filter the plus (+) syntax off and just send the email to `example@vertigion.com`.
I guess at that point you're no worse off than before &hellip;
- "Some mail servers violate RFC 5322, and the recommendations in RFC 3696, by refusing to send mail addressed to a user on another system merely because the local-part of the address contains the plus sign (+)" (Ref: [Wikipedia](http://en.wikipedia.org/wiki/E-mail_address#Sub-addressing)).
This has a few implications:
- Some mail servers will not send to your address with the plus (+) sign despite the [specifications](http://en.wikipedia.org/wiki/E-mail_address#RFC_specification).
- Some web sites may consider the plus (+) sign to be invalid despite the  [specifications](http://en.wikipedia.org/wiki/E-mail_address#RFC_specification).

# For those using Vertigion.com email &hellip;
 We support the plus (+) syntax, and will continue to support the plus (+) syntax after our email migration.
