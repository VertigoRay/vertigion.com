---
layout: post
title: PowerShell - Filter a list Computers by LastLogon Date
date: 2012-01-29 18:07
author: VertigoRay
comments: true
tags: [Function, posh, powershell, scripting, Uncategorized]
---
<p>I needed a script to filter a list of computers by the number of days since the computer was last logged onto.  This is my solution to that.  When supplied a list of computers, this will return the ones that have been Active (Logged On) within the Last <em>X</em> amount of days.  Where <em>X</em> is supplied as <em>$DaysSinceLastLogon</em>.  I&rsquo;ve populated the PowerShell Comment Doc, so check out the Examples supplied in there for usage.<!-- more --></p>
<pre class="brush: powershell">&lt;#
.SYNOPSIS
Filter a list Computers by LastLogon Date.
.DESCRIPTION
When supplied a list of computers, this will return the ones that have been Active (Logged On) within the Last X amount of days.  Where X is supplied as $DaysSinceLastLogon.
.PARAMETER ComputerNames
Object supplied via piped in result from a Get-ADComputer query, or similar.  Alias: Name
.PARAMETER DaysSinceLastLogon
Default: 180.  Number of days since Last Logon.  Used to justify an Active Computer.
.INPUTS
System.Object	ComputerNames
System.Int32	DaysSinceLastLogon
.OUTPUTS
List of Computer Names.
.EXAMPLE
Get-ADComputer Computer0123A | Get-CompByUser
Active Computers:  1
Total Computers:   1
The Active Computers are:

Hostname      LastLogon            OperatingSystem
--------      ---------            ---------------
Computer0123A 1/26/2012 8:55:20 PM Windows 7 Enterprise
.NOTES
.LINK
go.vertigion.com/PowerShell-Get-CompByActivity
function Get-CompByActivity {
	param(
		[Alias("Name")]
		[parameter(
			Position = 0,
			Mandatory = $true,
			ValueFromPipeline = $true,
			ValueFromPipelineByPropertyName = $true,
			HelpMessage = "Enter the Name of a computer account."
		)]
		$ComputerNames
		,
		[parameter(Mandatory = $false)]
		[int] $DaysSinceLastLogon = 180
	)

	Begin {
		$OutputList = @()
		$Domain = $(Get-ADDomain).DNSRoot
		$DCs = Get-ADDomainController
		$TotalComputers = 0
		
		function Measure-Latest {
			BEGIN { $latest = $null }
			PROCESS {if (($_ -ne $null) -and (($latest -eq $null) -or ($_ -gt $latest))) {$latest = $_}}
			END { $latest }
		}
	}
	
	Process {
		Write-Debug "ComputerNames: $ComputerNames"
		$TotalComputers += ($ComputerNames | Measure-Object).Count
		
		Write-Debug "ComputerNames Type:  $(($ComputerNames.GetType()).Name)"
		switch -regex ($(($ComputerNames.GetType()).Name)) {
			"PSCustomObject|ADComputer" {
				foreach ($Computer in $ComputerNames) {
					Write-Debug "Computer: $Computer"
					$LastLogon = [datetime]::FromFileTime($($(foreach ($DC in $DCs) {
						(Get-ADComputer $Computer.Name -Server $DC.Name -Properties LastLogon).LastLogon
					}) | Measure-Latest))
					if (($(Get-Date)-$LastLogon).Days -lt $DaysSinceLastLogon) {
						$NewObject = New-Object PSObject
						Add-Member -InputObject $NewObject NoteProperty 'Hostname' $Computer.Name.ToUpper()
						Add-Member -InputObject $NewObject NoteProperty 'LastLogon' $LastLogon
						Add-Member -InputObject $NewObject NoteProperty 'OperatingSystem' (Get-ADComputer ($Computer.Name) -Properties OperatingSystem).OperatingSystem
						$OutputList += $NewObject
					}
				}
			}
			default { Throw("Unable to handle that Type: $(($ComputerNames.GetType()).Name)") }
		}
	}
	
	End {
		Write-Debug "ComputerNames Type:  $(($ComputerNames.GetType()).Name)"
		Write-Output "Active Computers:  $($OutputList.Count)`nTotal Computers:   $TotalComputers`nThe Active Computers are:"
		Write-Output $OutputList | sort @{expression="LastLogon";Ascending=$true},@{expression="Hostname";Ascending=$true} | ft -auto
	}
}</pre>
<p>Hope it helps others out there.  Just add the function to your PowerShell Profile or as a Functions in a script to make it available!</p>
