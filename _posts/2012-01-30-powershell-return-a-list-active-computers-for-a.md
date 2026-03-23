---
layout: post
title: PowerShell - Return a list Active Computers for a Department/OU
date: 2012-01-30 18:03
author: VertigoRay
comments: true
tags: [Function, posh, powershell, scripting, Uncategorized]
---
<p>I needed a script that would return a list active computers for a supplied department or OU (basically the same thing in my use case).  This is my solution to that.  When supplied a list of Dept abbreviation (OU Name), this function will query AD for all enabled user accounts in the Departments OU.  Then it will run the list of users accounts through the <a href="http://go.vertigion.com/PowerShell-Get-CompByUser" title="PowerShell: Get-CompByUser" target="_self">Get-CompByUser</a> function.  The list of computers is then run through <a href="http://go.vertigion.com/PowerShell-Get-CompByActivity" title="PowerShell: Get-CompByActivity" target="_self">Get-CompByActivity</a> to return only active computers.  The actual targetted out will simply be a combination of $Dept and $OUBase parameters:  &quot;OU=$Dept,$OUBase&quot;.  I&rsquo;ve populated the PowerShell Comment Doc, so check out the Examples supplied in there for usage.<!-- more --></p>
<pre class="brush: powershell">&lt;#
.SYNOPSIS
Return a list Active Computers for a Department/OU.
.DESCRIPTION
Will query AD for all enabled User accounts in the Departments OU.  Will run the list of Users accounts through the Get-CompByUser function.  The list of computers is then run through Get-CompByActivity to return only active computers.

The actual targetted out will simply be a combination of $Dept and $OUBase:  "OU=$Dept,$OUBase"
.PARAMETER Dept
The abbreviated department name used to name the Department's Users OU.
.PARAMETER DaysSinceLastLogon
Default 180.  Number of days since Last Logon.  Used to justify an Active Computer.
.PARAMETER OUBase
Default "OU=Users,DC=VERTIGION,DC=COM".  The targetted base OU.
.INPUTS
System.String	DEPT/OU
.OUTPUTS
List of Active Computers for the supplied department/ou.
.EXAMPLE
Get-ActiveCompByDept DEPT
Active Computers:  3
Total Computers:   5
The Active Computers are:

Hostname      LastLogon             OperatingSystem
--------      ---------             ---------------
Computer0123A 1/26/2012 7:20:56 AM  Windows XP Professional
Computer0789A 1/26/2012 1:53:14 PM  Windows 7 Enterprise
Computer0123B 1/26/2012 8:56:14 PM  Windows 7 Enterprise
.NOTES
.LINK
go.vertigion.com/PowerShell-Get-ActiveCompByDept
#&gt;
function Get-ActiveCompByDept {
	param (
		[parameter(
			Position = 0,
			Mandatory = $true,
			HelpMessage = "Enter a Department Code (OU Name)"
		)]
		[string] $Dept
		,
		[int] $DaysSinceLastLogon = 180,
		[string] $OUBase = "OU=Users,DC=VERTIGION,DC=COM"
	)
	
	[string] $OU = "OU=$Dept,$OUBase"
	
	if ([adsi]::Exists("LDAP://$OU")) {
		$users = Get-ADUser -Filter {enabled -eq $true} -SearchBase $OU
		$Global:_ProgressTotal = ($users | Measure-Object).Count
		$users | Get-CompByUser -PassThru | ?{$_.Name -ne $null} | Get-CompByActivity -DaysSinceLastLogon $DaysSinceLastLogon
		Remove-Variable -Scope Global -Name _ProgressTotal
	} else {
		Throw("OU does not exist: $OU")
	}
}</pre>
<p>Clearly, you could modify this script to target a specific OU and get rid of the $Dept parameter.  Feel free to tweak it to suit your needs, and post back!</p>
<p>Hope it helps others out there.  Just add the function to your PowerShell Profile or as a Functions in a script to make it available!</p>
