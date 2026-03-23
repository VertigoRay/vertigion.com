---
layout: post
title: 'Multi-Factor Authentication: Switching Google Authenticator to Authy'
img: //i.imgur.com/FCGzAXT.png
author: VertigoRay
comments: true
tags:
- Authy
- Google
- Google Authenticator
- 2FA
- MFA
- LastPass
---
I’ve been a long time fan of Google Authenticator because Multi-Factor Authentication (MFA) is a must in today’s easily compromised society.
Unfortunately, Google Authenticator has been a struggle for many people, and I’m glad that I recently discovered Authy.

FYI: 2-Factor Authentication (2FA) is Multi-Factor Authentication.
However, not all MFA is 2FA.

# Google Authenticator Woes

Google Authenticator provides a neat way to use MFA.
But it has a massive downside that is mostly ignored.
If you lose/reset/replace your phone (which is normally your primary MFA device) then you’re likely completely out of luck.
This is why I started using Google Authenticator on my tablet as a backup.

Why?
Because all your MFA codes are gone, and never to be seen again.
The huge effort in recovering from this sort of mini-disaster makes me cry.

I’ve finally found the solution that will end these woes.
*Why has it taken me so long?*

# Authy App

[Authy](https://www.authy.com) is a fully-fledged MFA service.
But don’t get this confused with Google Authenticator.
They’re completely different.

What I’m referring to specifically is the **Authy App**.
[The Authy App also handles Google Authenticator MFA code registration.](https://authy.com/blog/authy-vs-google-authenticator/)
This means that instead of using the official Google app, you’ll now use the Authy App instead.

But isn’t the problem of your losing your phone exactly the same?
No; because with an Authy account you can now **securely backup** your Google Authenticator codes off your phone (to your Authy account via the app).
You read that right: You now have Google Authenticator backups!

What happens if you lose/reset your phone?
You just download the Authy App and retrieve your Google Authenticator codes from their backup.

It’s really as easy as that!

# LastPass Authenticator

[LastPass](https://lastpass.com/f?1006456) is a service that remembers your passwords so that you don’t have to.

The [LastPass](https://lastpass.com/f?1006456) service is a cloud-based password manager with extensions, mobile apps, and even desktop apps for all the browsers and operating systems you could want.
It’s extremely powerful and even offers a variety of [two-factor authentication options](https://helpdesk.lastpass.com/multifactor-authentication-options) so you can ensure no one else can log into your password vault.
[LastPass](https://lastpass.com/f?1006456) stores your passwords on [LastPass’s](https://lastpass.com/f?1006456) servers in an encrypted form – the [LastPass](https://lastpass.com/f?1006456) extension or app locally decrypts and encrypts them when you log in, so [LastPass](https://lastpass.com/f?1006456) couldn’t see your passwords if they wanted to.

[LastPass](https://lastpass.com/f?1006456) has recently launched [LastPass Authenticator](https://lastpass.com/auth).
Their app also backups all of your MFA tokens to your [LastPass](https://lastpass.com/f?1006456) account.
Keep in mind, I’m a huge fan of [LastPass](https://lastpass.com/f?1006456), but **it seems like a horribly bad idea to keep your passwords and MFA backups in the same service**.
With that said, I decided to go with Authy.

# Getting Setup in Authy

You will have to manually transfer all those Multi-Factor Authentication codes you currently have running on the original Google Authenticator app over to your new Authy app.

You can’t transfer them directly, so it’s more of a “turn it off and on again” process.
These are the basic steps:

For every Google Authenticator account you have:

Go to the original service for the account and remove Google Authenticator 2FA.
Re-enable Google Authenticator for that account
Use the Authy App instead of the Google Authenticator app to register the account.
It might be a bit tedious, but if you’ve already experienced the pain that comes with losing your GA codes, then you’ll agree some tedium is a cheap price to pay for the huge upside.

Don’t forget to turn on the app protection pin; under settings of the app.
This way, your MFA codes have that extra layer of protection from people you otherwise trust to peruse your phone.