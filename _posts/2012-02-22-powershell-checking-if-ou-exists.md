---
layout: post
title: PowerShell - Checking if OU exists
date: 2012-02-22 16:54
author: VertigoRay
img: //i.imgur.com/OGS4h59.jpg
comments: true
tags: 
- Active Directory
- AD
- LDAP
- OU
- posh
- powershell
redirect_from: /post/18075217070
---
After starting work on a function, I stumbled across [a StackOverflow article](http://stackoverflow.com/a/9399292/615422) and wanted to expand on that post.

That simple method only works well if the LDAP Path is [clean](http://lmgtfy.com/?q=What+is+clean+data?).
If you're possibly working with unclean data (or typo the DC structure), you'll need to catch your errors.<!-- more -->

```powershell
[string] $Path = 'OU=foo,OU=test,DC=domain,DC=com'
try {
	$ou_exists = [adsi]::Exists("LDAP://$Path")
} catch {
	# If invalid format, error is thrown.
	Throw("Supplied Path is invalid.`n$_")
}

if (-not $ou_exists) {
	Throw('Supplied Path does not exist.')
} else {
	Write-Debug "Path Exists:  $Path"
}
```

Since `DC=domain,DC=com` doesn't exist (or at least isn't accessible for an LDAP query), I get the following outputted from the above example; which is expected, since I threw the *Supplied Path is invalid* error (you can handle the error however you want):

```
Supplied Path is invalid.
Exception calling "Exists" with "1" argument(s): "A referral was returned from the server.
[...]
```

In my case, I only work within a particular OU in our Domain, so I've made it so my `$Path` can be abbreviated.
Here's how I handle things:

```powershell
[string] $RootOU = 'OU=test,DC=domain,DC=com'
[string] $Path = 'OU=foo'
try {
    $ou_exists = [adsi]::Exists("LDAP://$Path")
} catch {
    # If invalid format, error is thrown.
    Write-Debug "Supplied Path is invalid.`n$_"
    # It's probably the abbreviated version, so let's tack on the Root OU and confirm exists.
    Write-Debug 'Placing Path in Root OU and re-verifying ...'
    $Path = "$Path,$RootOU"
    try {
        $ou_exists = [adsi]::Exists("LDAP://$Path")
    } catch {
        Throw("Supplied Path is not valid, nor is our attempt to place it in the Root OU:`n$Path")
    }
}

if (-not $ou_exists) {
    Throw("Supplied Path does not exist:`n$Path")
} else {
    Write-Debug "Path Exists (1):  $Path"
}
```

If the `OU=foo,OU=test,DC=domain,DC=com` OU exists, the following will be outputed, in the above example:

```
DEBUG: Path Exists (2):  OU=foo,OU=test,DC=domain,DC=com
```

Hope this helps others out there.
Cheers!
