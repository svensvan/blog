---
layout: post
title: "Getting started with Cloudera SolrCloud"
---

Below are steps to create a new collection using the solrctl command, which comes with Cloudera's SolrCloud distro.

<pre class="prettyprint sh">
#Generate configuration files:
solrctl  --zk host1:2181,host2:2181/solr instancedir --generate myconf_local
</pre>

<pre class="prettyprint sh">
#Upload the new configuration files to Zookeeper:
solrctl  --zk host1:2181,host2:2181/solr instancedir --create myconf myconf_local
</pre>

<pre class="prettyprint sh">
#List the new configuration:
solrctl  --zk host1:2181,host2:2181/solr instancedir --list
</pre>

<pre class="prettyprint sh">
#Create a Solr collection with 2 shards:
solrctl  --zk host1:2181,host2:2181/solr collection --create mycoll -s 2 
</pre>

<pre class="prettyprint sh">
#View your new collection:
solrctl  --zk host1:2181,host2:2181/solr collection --list
</pre>

<pre class="prettyprint sh">
#Query the empty solr collection
curl http://localhost:8983/solr/mycoll/select?q=*:*
</pre>
