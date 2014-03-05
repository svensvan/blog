---
layout: post
title: "Important SolrCloud Commands"
---

Below are steps to create a new collection using solrctl, which is part of Cloudera's SolrCloud distro.

Generate configuration files
<pre class="prettyprint sh">
solrctl  --zk host1:2181,host2:2181/solr instancedir --generate myconf_local
</pre>

Upload the new configuration files to Zookeeper
<pre class="prettyprint sh">
solrctl  --zk host1:2181,host2:2181/solr instancedir --create myconf myconf_local
</pre>

List the new configuration
<pre class="prettyprint sh">
solrctl  --zk host1:2181,host2:2181/solr instancedir --list
</pre>

Create a Solr collection with 2 shards
<pre class="prettyprint sh">
solrctl  --zk host1:2181,host2:2181/solr collection --create mycoll -s 2 
</pre>

View your new collection
<pre class="prettyprint sh">
solrctl  --zk host1:2181,host2:2181/solr collection --list
</pre>
