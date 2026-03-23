---
layout: post
title: GPO - Not Applying User Policy
date: 2013-09-05 23:09
author: VertigoRay
comments: true
tags: [Active Directory, group policy, Uncategorized]
---
<p>We spent the day working on an issue where our User Policy wasn&rsquo;t being applied to the User AD Object logging into a computer. <span>We confirmed the usual steps:</span></p>
<ul><li>User AD Object is in the OU where the GPO is applied.</li>
<li>GPO is enabled.</li>
<li>Security Filtering is not filtering out the User AD Object.</li>
</ul><p>The policy was simply not showing up on the computer (would not display with `gpresult /r`).  After some more digging, we found the issue:<!-- more --></p>
<p><strong><span>One of the GPOs applied to the Computer AD Object had the </span><span class="explainlink"><em>Configure user Group Policy loopback processing mode</em> was set to <em>Replace</em>.</span></strong></p>
<p><span class="explainlink">Solution:</span></p>
<p><strong><span class="explainlink">Change </span><span class="explainlink"><em>Configure user Group Policy loopback processing mode</em> to <em>Merge</em>.</span></strong></p>
<p><em><strong>Note:</strong></em> our default has always been to use <em>Merge</em> in this setting &hellip; this one just slipped through the cracks. :(</p>
<h2>Explanation</h2>
<p><span class="explainlink">The GPO Explanation of the <em>Replace</em> setting says:</span></p>
<blockquote>
<p><span class="explainlink">&ldquo;Replace&rdquo; indicates that the user settings defined in the computer&rsquo;s Group Policy Objects replace the user settings normally applied to the user.</span></p>
</blockquote>
<p><span class="explainlink">To me, this means that Policies applied in the computer&rsquo;s GPO will overwrite the policies applied, but then we look at the GPO Explanation of the <em>Merge</em> setting:</span></p>
<blockquote>
<p><span class="explainlink">&ldquo;Merge&rdquo; indicates that the user setting defined in the computer&rsquo;s Group Policy  Objects and the user settings normally applied to the user are combined. If the settings conflict, the user settings in the computer&rsquo;s Group Policy Objects take precedence over the user&rsquo;s normal settings.</span></p>
</blockquote>
<p><span class="explainlink">Wait a minute!  That looks more like what I want!!  So what&rsquo;s the difference?  To find that answer, I dug in the <a href="http://support.microsoft.com/" target="_blank">Microsoft KB</a>.  The <a href="http://support.microsoft.com/kb/953768/en-us" title="User Group Policy loopback processing mode changes in Windows Vista, Windows Server 2008, Windows 7, and Windows Server 2008 R2" target="_blank">Win7 description of the setting</a> wasn&rsquo;t very useful, but the <a href="http://support.microsoft.com/kb/231287" title="Loopback processing of Group Policy" target="_blank">WinXP description of the setting</a> was the key to my complete understanding.  Quoted here for posterity (the <em>key</em> is bolded):</span></p>
<blockquote>
<p>Merge Mode:<br /><span>In this mode, when the user logs on, the user&rsquo;s list of GPOs is typically gathered by using the GetGPOList function. The GetGPOList function is then called again by using the computer&rsquo;s location in Active Directory. The list of GPOs for the computer is then added to the end of the GPOs for the user. This causes the computer&rsquo;s GPOs to have higher precedence than the user&rsquo;s GPOs. In this example, the list of GPOs for the computer is added to the user&rsquo;s list.</span></p>
<p>Replace Mode:<br /><span>In this mode, the <strong>user&rsquo;s list of GPOs is not gathered</strong>. Only the list of GPOs based on the computer object is used.</span></p>
</blockquote>
<p>So when do you want to use <em>Replace</em> mode?</p>
<p>A quick scenario might be at a lab computer at a school that allows users to login with their AD credentials instead of using the auto-login account.  You may not want the settings that you allow to be loaded at a user&rsquo;s office computer to be pulled down to the lab computer.</p>
