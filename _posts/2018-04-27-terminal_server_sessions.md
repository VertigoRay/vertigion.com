---
layout: post
title: Query Terminal Server Sessions
author: VertigoRay
img: //i.imgur.com/OGS4h59.jpg
comments: true
tags:
- PowerShell
- PoSh
---
I wanted a quick way to get a list of user sessions on our VDI servers.
My exact goal was to get a list of users that were a day away from being forced off the system and sending them an email.

**TLDR**: I ended up making this into a module: [QuserObject](/2018/07/22/quserobject).

# Walkthrough

Anyway, down to business.
We can use [Quser](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/cc754583(v=ws.11)) to get the information.
Keep in mind that you can easily target a different computer: `quser /SERVER:servername`.

```cmd
USERNAME         SESSIONNAME  ID  STATE   IDLE TIME  LOGON TIME
 admin.xxxxxxxx                2  Disc     15+15:12  7/20/2017 1:19 PM
 admin.xxxxx      rdp-tcp#54   3  Active       1:39  7/21/2017 5:35 AM
 xxxxxxxx                      4  Disc      6+04:10  7/21/2017 9:25 AM
>vertigoray          console   5  Active          .  8/9/2017 4:40 PM
```

This is good information, but it would be so much better if it was an object.
Since it's already a nice grid, let's treat it as a CSV:

```powershell
PS > (quser) -replace '\s{2,}', ',' | ConvertFrom-Csv

USERNAME    : admin.xxxxxxxx
SESSIONNAME : 2
ID          : Disc
STATE       : 15+15:12
IDLE TIME   : 7/20/2017 1:19 PM
LOGON TIME  :

USERNAME    : admin.xxxxx
SESSIONNAME : rdp-tcp#54
ID          : 3
STATE       : Active
IDLE TIME   : 1:39
LOGON TIME  : 7/21/2017 5:35 AM

USERNAME    : xxxxxxxx
SESSIONNAME : 4
ID          : Disc
STATE       : 6+04:10
IDLE TIME   : 7/21/2017 9:25 AM
LOGON TIME  :

USERNAME    : >vertigoray
SESSIONNAME : console
ID          : 5
STATE       : Active
IDLE TIME   : .
LOGON TIME  : 8/9/2017 4:40 PM
```

Beautifully simple, right?!
I originally [posted this on stackoverflow](https://stackoverflow.com/a/49042770/615422) because I just wanted a list of users.

Now, let's get rid of that `>` on my entry, and fix those disconnected session columns.
This part gets a little more complicated.

```powershell
(((quser) -replace '^>', '') -replace '\s{2,}', ',').Trim() | ForEach-Object {
    if ($_.Split(',').Count -eq 5) {
        Write-Output ($_ -replace '(^[^,]+)', '$1,')
    } else {
        Write-Output $_
    }
} | ConvertFrom-Csv
```

**Output:**

```
USERNAME    : admin.xxxxxxxx
SESSIONNAME :
ID          : 2
STATE       : Disc
IDLE TIME   : 15+15:12
LOGON TIME  : 7/20/2017 1:19 PM

USERNAME    : admin.xxxxx
SESSIONNAME : rdp-tcp#54
ID          : 3
STATE       : Active
IDLE TIME   : 1:39
LOGON TIME  : 7/21/2017 5:35 AM

USERNAME    : xxxxxxxx
SESSIONNAME :
ID          : 4
STATE       : Disc
IDLE TIME   : 6+04:10
LOGON TIME  : 7/21/2017 9:25 AM

USERNAME    : vertigoray
SESSIONNAME : console
ID          : 5
STATE       : Active
IDLE TIME   : .
LOGON TIME  : 8/9/2017 4:40 PM
```

Last things to do:

- Change the idle `.` to a `$null`.
- Type cast everything appropriately.
- Change the property names to PascalCase.

Here's the code for that:

```powershell
$script:headerRowProcessed = $false
(((quser) -replace '^>', '') -replace '\s{2,}', ',').Trim() | ForEach-Object {
    if ($_.Split(',').Count -eq 5) {
        Write-Output ($_ -replace '(^[^,]+)', '$1,')
    } else {
        Write-Output $_
    }
} | ForEach-Object {
    $rowParts = $_.Split(',')
    if (-not $script:headerRowProcessed) {
        [System.Collections.ArrayList] $parts = @()
        foreach ($part in $rowParts) {
            $parts.Add((Get-Culture).TextInfo.ToTitleCase($part.ToLower()).Replace(' ', '')) | Out-Null
        }
        Write-Output ($parts -join ',')
        $script:headerRowProcessed = $true
    } else {
        [int] $rowParts[2] = $rowParts[2]
        
        if ($rowParts[3] -eq 'Disc') {
            $rowParts[3] = 'Disconnected'
        }
        
        if ($rowParts[4] -eq '.') {
            $rowParts[4] = $null
        } else {
            $parts = $rowParts[4].Split('+:')
            $now = Get-Date
            if ($parts.Count -eq 3) {
                $rowParts[4] = $now.AddDays(-1 * $parts[0]).AddMinutes(-1 * $parts[1]).AddSeconds(-1 * $parts[2])
            } else {
                $rowParts[4] = $now.AddMinutes(-1 * $parts[0]).AddSeconds(-1 * $parts[1])
            }
        }
        
        $rowParts[5] = Get-Date $rowParts[5]

        Write-Output ($rowParts -join ',')
    }
} | ConvertFrom-Csv
```

**Output:**

```
Username    : admin.xxxxxxxx
Sessionname :
Id          : 2
State       : Disconnected
IdleTime    : 07/25/2017 16:24:48
LogonTime   : 07/20/2017 13:19:00

Username    : admin.xxxxx
Sessionname : rdp-tcp#54
Id          : 3
State       : Active
IdleTime    : 08/09/2017 16:38:21
LogonTime   : 07/21/2017 05:35:00

Username    : xxxxxxxx
Sessionname :
Id          : 4
State       : Disconnected
IdleTime    : 08/03/2017 16:35:50
LogonTime   : 07/21/2017 09:25:00

Username    : vertigoray
Sessionname : console
Id          : 5
State       : Active
IdleTime    :
LogonTime   : 08/09/2017 16:40:00
```

# Conclusion

I bit more complex, but still beautiful, right?
~~I'll have to turn that into a module for ease of use.~~
I ended up making this into a module: [QuserObject](/2018/07/22/quserobject).
