---
layout: post
title: PowerShell - Random Color
date: 2012-02-15 17:03
author: VertigoRay
comments: true
tags: [Colors, posh, powershell, scripting, Uncategorized]
---
<p>Just messing around here, but thought others might want to know how to randomly (or intelligently) grab a color.  There&rsquo;s basically three parts to this process.</p>
<ol><li>Get the number of colors.</li>
<li>Grab a random color.</li>
<li>Apply the color.</li>
</ol><p>I&rsquo;ve done the above three steps in these three lines of code &hellip;<!-- more --></p>
<pre class="brush: powershell">$max = [System.ConsoleColor].GetFields().Count - 1
$color = [System.ConsoleColor](Get-Random -Min 0 -Max $max)
Write-Host -Fore $color 'lolz'</pre>
<p>If you want the one liner version &hellip;</p>
<pre class="brush: powershell">Write-Host -Fore ([System.ConsoleColor](Get-Random -Min 0 -Max ([System.ConsoleColor].GetFields().Count - 1))) 'lolz'</pre>
<p>Want a list of colors?</p>
<pre class="brush: powershell">[System.ConsoleColor].GetFields() | %{$_.Name}
</pre>
<p>The first thing returned <em>value__</em> is just metadata and won&rsquo;t be randomly selected in the range we&rsquo;ve specified.  If we randomly get <em>0</em>, <em>Black</em> is returned.  If we randomly get <em>15</em>, <em>White</em> is returned.</p>
<p>Note:  As of the time of this writing, <em>.Count-1</em> from the above code equals <em>16</em>.  Get-Random&rsquo;s <em>-Maximum</em> parameter &ldquo;returns a value that is less than the maximum (not equal).&rdquo;  Thus, our inclusive range is 0 through 15.</p>
<p>Hope this helps others out there.  Cheers!</p>
