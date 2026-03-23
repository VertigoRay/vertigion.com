---
layout: post
title: Installing and configuring Munki with Puppet
date: 2013-07-25 22:56
author: VertigoRay
comments: true
tags: [Uncategorized]
---
<p>&ldquo;<a href="http://projects.puppetlabs.com/projects/puppet" title="Puppet" target="_blank">Puppet</a> is a powerful system administration tool for Macs. But <a href="http://code.google.com/p/munki/" title="Munki" target="_blank">Munki</a> is better at managing software packages. Do the smart thing and use puppet to deploy and configure Munki on your Mac clients.&rdquo; &ndash; <a href="http://seeskill.wordpress.com/author/cilefen/" title="Chris McCafferty" target="_blank">Chris McCafferty</a></p>
<p>I couldn&rsquo;t have said it better myself. Thanks for the recipe! <span>Here&rsquo;s my version of the same solution &hellip;</span><!-- more --></p>
<p>Place the following code in your <em>modules/munki/manifests</em>/<em>init.pp</em> file:</p>
<pre class="brush: puppet">class munki {
<span>	</span># <a href="http://go.vertigion.com/PuppetModules-Munki">http://go.vertigion.com/PuppetModules-Munki</a>
<span>	</span>$munki = 'munkitools-0.9.0.1803.0'
	$munki_download = "https://munki.googlecode.com/files/$munki.dmg"
	$munki_server = 'http://munki.example.com'

	# <a href="https://code.google.com/p/munki/wiki/BootstrappingWithMunki">https://code.google.com/p/munki/wiki/BootstrappingWithMunki</a>
	$bootstrap = '/usr/bin/touch /Users/Shared/.com.googlecode.munki.checkandinstallatstartup'
	$managed_installs = '/Library/Preferences/ManagedInstalls'

	package { "$munki.dmg" :
		provider =&gt; pkgdmg,
		alias =&gt; 'munkitools',
		ensure =&gt; installed,
		source =&gt; $munki_download,
		notify =&gt; Exec[$bootstrap],
	}

	exec {$bootstrap :
		refreshonly =&gt; true,
		notify =&gt; Exec['/sbin/reboot']
	}

	exec {'/sbin/reboot' :
		refreshonly =&gt; true,
	}

	file {"$managed_installs.plist" :
		ensure =&gt; present,
		owner =&gt; root,
		group =&gt; admin,
	}

	exec {"/usr/bin/defaults write $managed_installs SoftwareRepoURL "$server/munki_repo"" :
		before =&gt; Package['munkitools'],
		require =&gt; File["$managed_installs.plist"],
	}

	exec {"/usr/bin/defaults write $managed_installs ClientIdentifier "workstation"" :
		before =&gt; Package['munkitools'],
		require =&gt; File["$managed_installs.plist"],
	}

	exec {"/usr/bin/defaults write $managed_installs InstallAppleSoftwareUpdates -bool True" :
		before =&gt; Package['munkitools'],
		require =&gt; File["$managed_installs.plist"],
	}
}</pre>
