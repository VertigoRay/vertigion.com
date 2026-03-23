---
layout: post
title: Loop Through Objects
author: Raymond Piller
img: //i.imgur.com/OGS4h59.jpg
comments: true
tags:
- PowerShell
- PoSh
---
Looping through objects can be a pain, until you know what you're doing.
First thing you need to do is determine what kind of object you're dealing with

- [Array](#array)
- [Hashtable](#hashtable)
    - [Use `GetEnumerator()`](#use-getenumerator)
    - [Use `Keys`](#use-keys)
- [PSObject](#psobject)
    - [Use `PSObject.Properties`](#use-psobjectproperties)
    - [Practical Example](#practical-example)
- [Additional information](#additional-information)

*Note: if it's anything besides the above types, it's like a child of a PSObject, and you can use that method.*

Before I dive into this I'll state that shorthand is not preferred for scripts; it's less readable.
The `%` operator is considered shorthand of `ForEach-Object`.
This was the only time you will see a percent sign in this post.

In scripts, I prefer to use `foreach` instead of `ForEach-Object`.
Unless, of course, there's a speed benefit; such as using workflows, executing commands across several computers, etc.
[Speed increases aren't always realized.](https://gist.github.com/VertigoRay/3bb0166d6a877839b420)
Regardless, my decision is because readability is lost by using the automatic, implied `$_` variable.

# Array

Arrays are pretty simple, but I'll cover them anyway for thoroughness.

```powershell
[array] $arr = @(1, 2, 3)

foreach ($item in $arr) {
    Write-Host "Item: ${item}"
}
```

**Output:**

```powershell
Item: 1
Item: 2
Item: 3
```

**`ForEach-Object` example:**

```powershell
$arr | ForEach-Object {
    Write-Host "Item: ${_}"
}
```

*Same output as previous example.*

# Hashtable

[*I originally posted this on stackoverflow.*](https://stackoverflow.com/a/16175967/615422)
I'll cover a couple of options, but they'll use the same variable:

```powershell
$hash = @{
    a = 1
    b = 2
    c = 3
}
```

## Use `GetEnumerator()`

*Note: personal preference; syntax is easier to read for someone that's new to PowerShell.*

The GetEnumerator() method would be done as shown:

```powershell
foreach ($h in $hash.GetEnumerator()) {
    Write-Host "$($h.Name): $($h.Value)"
}
```

**Output:**

```powershell
c: 3
b: 2
a: 1
```

**`ForEach-Object` example:**

```powershell
$hash.GetEnumerator() | ForEach-Object {
    Write-Host "$($_.Name): $($_.Value)"
}
```

*Same output as previous example.*

## Use `Keys`

The Keys method would be done as shown:

```powershell
foreach ($h in $hash.Keys) {
    Write-Host "${h}: $($hash.Item($h))"
}
```

**Output:**

```powershell
c: 3
b: 2
a: 1
```

**`ForEach-Object` example:**

```powershell
$hash.Keys | ForEach-Object {
    Write-Host "${_}: $($hash.Item($_))"
}
```

*Same output as previous example.*

# PSObject

I'll cover a couple of options, and they'll use the same variable structure as the hashtable:

```powershell
$pso = New-Object PSObject                                       
$Object | Add-Member NoteProperty a 1
$Object | Add-Member NoteProperty b 2
$Object | Add-Member NoteProperty c 3
```

That's kind of a pain, so I'll show you an easier way that been available since PowerShell 2.0:

```powershell
$pso = New-Object PSObject -Property @{
    a = 1
    b = 2
    c = 3
}
```

## Use `PSObject.Properties`

The `PSObject` property is available, albeit hidden, on all PS Objects.

```powershell
foreach ($item in $pso.PSObject.Properties) {
    Write-Host "$($item.Name): $($item.Value)"
}
```

**Output:**

```powershell
c: 3
b: 2
a: 1
```

**`ForEach-Object` example:**

```powershell
$pso.PSObject.Properties | ForEach-Object {
    Write-Host "${_}: $($hash.Item($_))"
}
```

## Practical Example

[*I originally posted this on stackoverflow.*](https://stackoverflow.com/a/33792068/615422)

```powershell
$ipinfo = Invoke-WebRequest 'http://ipinfo.io/json' -UseBasicParsing | ConvertFrom-Json
foreach ($info in $ipinfo.PSObject.Properties) {
    Write-Host "$($info.Name): $($info.Value)"
}
```

**Outputs:**

```powershell
ip: 1.2.3.4
hostname: No Hostname
city: Denton
region: Texas
country: US
loc: 33.2148,-97.1331
org: AS589 University of North Texas
postal: 76203
```

*This example was originally done with POSH 4.0; `ConvertTo-Json` and `ConvertFrom-Json` were introduced in POSH 3.0. I haven't tested POSH 3.0. I have tested in PowerShell 5.0 and 5.1; I got the same output.*

**`ForEach-Object` example:**

```powershell
(Invoke-WebRequest 'http://ipinfo.io/json' -UseBasicParsing | ConvertFrom-Json).PSObject.Properties | ForEach-Object {
    Write-Host "$($_.Name): $($_.Value)"
}
```

*Same output as previous example.*

# Additional information

Be careful sorting your hashtable...
[Sort-Object](http://technet.microsoft.com/en-us/library/ee176968) may change it to an array:

```powershell
PS> $hash.GetType()

IsPublic IsSerial Name      BaseType
-------- -------- ----      --------
True     True     Hashtable System.Object


PS> $hash = $hash.GetEnumerator() | Sort-Object Name
PS> $hash.GetType()

IsPublic IsSerial Name     BaseType
-------- -------- ----     --------
True     True     Object[] System.Array
```