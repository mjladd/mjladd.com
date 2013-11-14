---
layout: post
title: "Changing Puppet Config Paths Under Passenger"
tags:
- tech
- puppet
---
{% include JB/setup %}

Making some progress in getting our puppet manifests under a capistrano script. Everything has been under version control for awhile, but it's been my plan to get a proper deployment configuration set up. 

Before I could get capistrano set up, I needed to make sure that puppet was running out of a new path, since cap is going to be removing and recreating symlinks. 

`mkdir /etc/puppet/current` <br>
`mv /etc/puppet/* /etc/puppet/current`

/etc/puppet/puppet.conf<br>
`[main]`<br>
`    tagmap = /etc/puppet/current/tagmail.conf` <br>
`[master]`<br>
`    modulepath = /etc/puppet/current/modules`<br>

The missing piece was remembering that we run the puppetmaster out of passenger and passenger was not finding the path changes. 

Adding the following to:<br>
`/usr/share/puppet/rack/puppetmaster/config.ru`<br>
`ARGV << "--confdir=/etc/puppet/current"`<br>

A restart of httpd and all was well. The puppetmaster was running out of the new configuration path. The next step is to deploy the configuration via capistrano.
