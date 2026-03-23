---
layout: post
title: PowerShell - Pick a Domain Controller
date: 2012-02-02 18:02
author: VertigoRay
comments: true
tags: [Domain Controller, Get-ADDomainController, Global Variable, posh, powershell, Scriptin, Uncategorized]
---
<p>I use a global variable to pick a Domain Controller so that I&rsquo;m not constantly bouncing back and forth between Domain Controllers when running scripts. This helps to mitigate errors due to replication delays (normally, just a few seconds).</p>
<p>For Example, when you&rsquo;re scripting and try to run <em>Set-ADUser</em> command followed immediately by a <em>Get-ADUser</em> command without specifying the <em>-Server</em> property as the same server for both commands, you may end up setting the change on one server and confirming the change was set (possibly a nano-second later) on a different server. This would lead to your confirmation to return as false.<!-- more --></p>
<p>To draw it out a little clearer &hellip; take the following command:</p>
<pre class="brush: powershell">While ((Get-ADUser $SamAccountName -Properties HomeDrive).HomeDrive -eq $null) {
	Write-Host "Setting HomeDrive &amp; Home Directory ..."
	Set-ADUser $SamAccountName -HomeDrive "H:" -HomeDirectory "\HOME-SERVERHOME$SamAccountName"
}
Write-Host "Moving on ..."</pre>
<p>Without specifying <em>-Server</em>, the command might result in the following output:</p>
<pre class="brush: powershell">Setting HomeDrive &amp; Home Directory ...
Setting HomeDrive &amp; Home Directory ...
Setting HomeDrive &amp; Home Directory ...
Setting HomeDrive &amp; Home Directory ...
Moving on ...</pre>
<p>If I do specify -Server, as shown:</p>
<pre class="brush: powershell">While ((Get-ADUser $SamAccountName -Server $myDC -Properties HomeDrive).HomeDrive -eq $null) {
	Write-Host "Setting HomeDrive &amp; Home Directory ..."
	Set-ADUser $SamAccountName -Server $myDC -HomeDrive "H:" -HomeDirectory "\HOME-SERVERHOME$SamAccountName"
}
Write-Host "Moving on ..."</pre>
<p>I get the following output, everytime:</p>
<pre class="brush: powershell">Setting HomeDrive &amp; Home Directory ...
Moving on ...</pre>
<p>I set $myDC in my $Profile with a global variable and calling the Set-DC function I&rsquo;ve written below.</p>
<pre class="brush: powershell">[string] $global:myDC = ""
Set-myDC
Write-Debug "myDC:  $myDC"</pre>
<p>Of course, you could just set the $myDC statically, but what fun is that? Here&rsquo;s my solution:</p>
<pre class="brush: powershell">&lt;#
.SYNOPSIS
Sets the myDC global variable.
.DESCRIPTION
Gets a list of domain controllers and takes one from the list.
.PARAMETER Return
If set, will return the FQDN of a DC, other than the globally set DC, intead of setting it globally.  Useful for targetting another DC without changing the globally used DC.
.INPUTS
.OUTPUTS
Sets $global:CASITS_UNTDC discovered FQDN.
If -Return is used, $global:myDC is not set and the discovered FQDN is returned as a string.
.EXAMPLE
Set-myDC
.EXAMPLE
$myDC = Set-myDC -Return
.NOTES
.LINK
go.vertigion.com/PowerShell-Set-myDC
#&gt;
function Set-myDC
{
	param(
        [Parameter(
			HelpMessage = "If set, will return the result intead of setting globally."
		)]
        [switch] $Return
    )
	
	$DCList = $null
	while ($DCList -eq $null) {
		$DCList = Get-ADDomainController -Filter * | Select HostName
	}
	if ($Return.isPresent) {
		$DC = ''
		while ($DC -eq $myDC) {
			$DC = $DCList[$(Get-Random -Minimum 0 -Maximum ($DCList | Measure-Object).Count)].HostName
		}
		return $DC
	} else {
		$global:myDC = $DCList[$(Get-Random -Minimum 0 -Maximum ($DCList | Measure-Object).Count)].HostName
	}
}</pre>
<p>Hope it helps others out there. Just add the function to your PowerShell Profile or as a Functions in a script to make it available!</p>
