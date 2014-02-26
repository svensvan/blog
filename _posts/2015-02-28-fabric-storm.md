---
layout: post
title: "Using Fabric to Automate Tasks for Storm"
---

A Storm cluster has many nodes.  To automate the steps for creating and managing nodes use something like Python Fabric. Listed below is the fabfile.py based on
	[Michael Noll's excellent tutorial](http://www.michael-noll.com/tutorials/running-multi-node-storm-cluster/ "Storm Tutorial")


<pre class="prettyprint Python">
#!/usr/bin/env/python
from fabric.api import *

def create_storm_user():
	sudo('groupadd -g 53001 storm')
	sudo('mkdir -p /app/home')
	sudo('useradd -u 53001 -g 53001 -d /app/home/storm')
	sudo('chmod 700 /app/home/storm')

def setup_storm():
	put('/opt/storm.tar.gz', '/opt')
	run('tar -xzvf /opt/storm.tar.g -C /usr/local')
	run('chown -R storm:storm /usr/local/storm')
	sudo('mkdir -p /app/storm')
	run('chown -R storm:storm /app/storm')
	run('chmod 750 /app/storm')

def setup_zeromq():
	put('/root/dev/zeromq-2.1.7-1.el6.x86_64.rpm', '/opt')
	put('/root/dev/jzmq-2.1.0.el6.x86_64.rpm', '/opt')
	run('yum install zeromq')
	run('yum install jzmq')
</pre>

