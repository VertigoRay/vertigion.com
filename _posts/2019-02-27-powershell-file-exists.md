---
layout: post
title: 'PowerShell: Check if a File Exists'
author: VertigoRay
img: //i.imgur.com/OGS4h59.jpg
comments: true
tags:
- PowerShell
- PoSh
---
There are many ways to skin a cat, and with PowerShell, that's no exception.
I'm going to show you three ways that I'm familiar with to ~~skin~~ check if a file exists.

# Test-Path

This is probably the most common, and works well for a quick and simple solution.

```powershell
PS > Remove-Item 'C:\Temp\*'
PS > Get-ChildItem 'C:\Temp'
PS > Test-Path 'C:\Temp\foo.txt'
False
PS > New-TemporaryFile | Move-Item -Destination 'C:\Temp\foo.txt'
PS > Test-Path 'C:\Temp\foo.txt'
True
```

I prefer quotes around strings like paths for cleanliness and readability in scripts, but you don't need the quotes around the path names on any of those commands.
I don't use quotes in the command line for speed. Powershell is expecting a string so it'll figure it out:

```powershell
PS > Remove-Item C:\Temp\*
PS > Get-ChildItem C:\Temp
PS > Test-Path C:\Temp\foo.txt
False
PS > New-TemporaryFile | Move-Item -Destination C:\Temp\foo.txt
PS > Test-Path C:\Temp\foo.txt
True
```

This can also be used with piping.
However, when piping, you definitely need the quotes because PowerShell has no expectations.
This is another reason I prefer to just use quotes all the time:

```powershell
PS > C:\Temp\foo.txt | Test-Path
Cannot run a document in the middle of a pipeline: C:\Temp\foo.txt.
At line:1 char:1
+ C:\Temp\foo.txt | Test-Path
+ ~~~~~~~~~~~~~~~
+ CategoryInfo          : InvalidOperation: (C:\Temp\foo.txt:String) [], RuntimeException
+ FullyQualifiedErrorId : CantActivateDocumentInPipeline

PS > 'C:\Temp\foo.txt' | Test-Path
True
```

If you want to test if there are any `.txt` files in a path or any files with a name that starts with a letter, this is doable with this function using a wildcard:

```powershell
PS > Test-Path 'C:\Temp\*.txt'
True
PS > Test-Path 'C:\Temp\f*'
True
PS > Test-Path 'C:\Temp\f*.txt'
True
PS > Test-Path 'C:\Temp\bar*'
False
```

# [System.IO.File]::Exists()

:notebook:
`[System]` is the default root type, so things like `[System.String]` can be shortcutted to `[String]`.
I'll be shortcutting `[System.IO.File]` to just `[IO.File]`.

An alternative you can use is the `[System.IO.File]::Exists()` which access the .NET method. This is the simplest (aka least feature rich) test, and I'm not really sure why you'd use this over `Test-Path`. However, it's here for completeness.

```powershell
PS > Remove-Item C:\Temp\*
PS > Get-ChildItem C:\Temp\
PS > [System.IO.File]::Exists('C:\Temp\foo.txt')
False
PS > New-TemporaryFile | Move-Item -Destination 'C:\Temp\foo.txt'
PS > [System.IO.File]::Exists('C:\Temp\foo.txt')
True
```

Wildcard tests are not supported by this .NET method.

# [System.IO.FileInfo]

:notebook:
`[System]` is the default root type, so things like `[System.String]` can be shortcutted to `[String]`.
I'll be shortcutting `[System.IO.FileInfo]` to just `[IO.FileInfo]`.

Using the `[IO.FileInfo]` type is my personal preference when working with files because it gives me a ton of information about the file with a single call.
Information that I would have to use a host of tools to get, including:

- `Get-Item`
- `Get-ItemProperty`
- `Split-Path`
- `Test-Path`
- *etc*

```powershell
PS > Remove-Item C:\Temp\*
PS > Get-ChildItem C:\Temp\
PS > [IO.FileInfo] $foo = 'C:\Temp\foo.txt'
PS > $foo.Exists
False
PS > New-TemporaryFile | Move-Item -Destination C:\Temp\foo.txt
PS > $foo.Exists
False
PS > $foo.Refresh()
PS > $foo.Exists
True
```

Using the `[IO.FileInfo]` type, you have the added benefit of a bunch of additional file information; go figure:

```powershell
PS > $foo | Select-Object *


Mode              : -a----
VersionInfo       : File:             C:\Temp\foo.txt
                    InternalName:
                    OriginalFilename:
                    FileVersion:
                    FileDescription:
                    Product:
                    ProductVersion:
                    Debug:            False
                    Patched:          False
                    PreRelease:       False
                    PrivateBuild:     False
                    SpecialBuild:     False
                    Language:

BaseName          : foo
Target            : {}
LinkType          :
Length            : 0
DirectoryName     : C:\Temp
Directory         : C:\Temp
IsReadOnly        : False
FullName          : C:\Temp\foo.txt
Extension         : .txt
Name              : foo.txt
Exists            : True
CreationTime      : 2/27/2019 8:57:33 AM
CreationTimeUtc   : 2/27/2019 1:57:33 PM
LastAccessTime    : 2/27/2019 8:57:33 AM
LastAccessTimeUtc : 2/27/2019 1:57:33 PM
LastWriteTime     : 2/27/2019 8:57:33 AM
LastWriteTimeUtc  : 2/27/2019 1:57:33 PM
Attributes        : Archive
```

Additionaly, `DirectoryName` is a `[String]`, but `Directory` is an `[IO.DirectoryInfo]` type:

```powershell
PS > $foo.Directory | Select-Object *


Mode              : d-----
BaseName          : Temp
Target            : {}
LinkType          :
Parent            : C:\
Root              : C:\
FullName          : C:\Temp
Extension         :
Name              : Temp
Exists            : True
CreationTime      : 1/15/2019 12:25:37 PM
CreationTimeUtc   : 1/15/2019 5:25:37 PM
LastAccessTime    : 2/27/2019 9:33:30 AM
LastAccessTimeUtc : 2/27/2019 2:33:30 PM
LastWriteTime     : 2/27/2019 9:33:30 AM
LastWriteTimeUtc  : 2/27/2019 2:33:30 PM
Attributes        : Directory
```

Also, that object's `Parent` property is also an `[IO.DirectoryInfo]` type:

```powershell
PS > $foo.Directory.Parent | Select-Object *


Mode              : d--hs-
BaseName          : C:\
Target            : {}
LinkType          :
Parent            :
Root              : C:\
FullName          : C:\
Extension         :
Name              : C:\
Exists            : True
CreationTime      : 3/18/2017 6:40:20 AM
CreationTimeUtc   : 3/18/2017 11:40:20 AM
LastAccessTime    : 2/25/2019 10:03:41 AM
LastAccessTimeUtc : 2/25/2019 3:03:41 PM
LastWriteTime     : 2/25/2019 10:03:41 AM
LastWriteTimeUtc  : 2/25/2019 3:03:41 PM
Attributes        : Hidden, System, Directory
```

Sure, you could just `cd ..` to get up a directory and get higher level information.
What if you're programmatically looking through files at a location that's not your present working directory (`$pwd`)?
You can easily pull the file's version of `cd ..` with this:

```powershell
PS > $foo.Directory.Parent

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d--hs-        2/25/2019  10:03 AM                C:\
```

If we were deeper in, we could traverse as high as we want:

```powershell
PS > [void] (New-Item -Type Directory -Path 'C:\Temp\a\b\c\d\e\f\g' -Force)
PS > New-TemporaryFile | Move-Item -Destination 'C:\Temp\a\b\c\d\e\f\g\foo.txt'
PS > [IO.FileInfo] $foo = 'C:\Temp\a\b\c\d\e\f\g\foo.txt'
PS > $foo.Directory.Parent.Parent.Parent.Parent.Parent | Select-Object *


Mode              : d-----
BaseName          : b
Target            : {}
LinkType          :
Parent            : C:\Temp\a
Root              : C:\
FullName          : C:\Temp\a\b
Extension         :
Name              : b
Exists            : True
CreationTime      : 2/27/2019 9:46:26 AM
CreationTimeUtc   : 2/27/2019 2:46:26 PM
LastAccessTime    : 2/27/2019 9:47:28 AM
LastAccessTimeUtc : 2/27/2019 2:47:28 PM
LastWriteTime     : 2/27/2019 9:46:26 AM
LastWriteTimeUtc  : 2/27/2019 2:46:26 PM
Attributes        : Directory
```