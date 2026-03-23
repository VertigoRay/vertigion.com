---
layout: post
title: SCCM - Query/Collection by MSI Installed
date: 2012-02-22 18:03
author: VertigoRay
comments: true
tags: [Active Directory, AD, Collections, Java, MSI, SCCM, Uncategorized]
---
<p>Most of the time, when adding an SCCM Collection based on installed software, I&rsquo;ll simply use the <em>Add/Remove Programs</em> Attribute Class.  However, in a recent case, the installed software doesn&rsquo;t show up in <em>Add/Remove Programs</em>, but it is an MSI installer.  In this particular instance, I wanted to remove the Java Updater.<!-- more --></p>
<p>Since approximately 1.6.18 / 6.0 update 18, Oracle (formerly Sun) has included Java Auto Updater as a separate package that is automatically installed with the JRE <a href="http://wpkg.org/Java#JRE_6_Update_18_and_Newer" title="wpkg.org" target="_blank">[ref]</a>.  This makes it really easy to remove:</p>
<pre>msiexec /qn /x {4A03706F-666A-4037-7777-5F2748764D10}</pre>
<p>Now, I just needed to figure out how to target all computers with that package code installed.  After a lot of Googling, and turning up nothing useful, I decided to dig into SCCM myself.  Here&rsquo;s how to set-up the <a href="http://technet.microsoft.com/en-us/library/bb693909.aspx" title="TechNet" target="_blank">Criterion Properties</a>, <a href="http://technet.microsoft.com/en-us/library/bb632615.aspx" title="TechNet" target="_blank">for a Collection</a>, to look for machines with <em>Java Updater</em> installed:</p>
<p><img alt="SCCM Criterion Properties" height="530" src="http://images.vertigion.com/blog/SCCM%20-%20Query%20by%20MSI%20Installed/SCCM-Collection-MSI.png" width="478" /></p>
<p>If you&rsquo;d rather just have the SQL, here&rsquo;s what my system generated:</p>
<pre>select SMS_R_System.ResourceId, SMS_R_System.ResourceType, SMS_R_System.Name, SMS_R_System.SMSUniqueIdentifier, SMS_R_System.ResourceDomainORWorkgroup, SMS_R_System.Client from  SMS_R_System inner join SMS_G_System_INSTALLED_SOFTWARE on SMS_G_System_INSTALLED_SOFTWARE.ResourceID = SMS_R_System.ResourceId where UPPER(SMS_G_System_INSTALLED_SOFTWARE.ProductCode) = "{4A03706F-666A-4037-7777-5F2748764D10}"</pre>
<p>Hope this helps others out there.  Cheers!</p>
