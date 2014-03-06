---
layout: post
title: "Getting started with SolrCloud"
---

The steps below will create a new Solr collection. 

<pre class="prettyprint sh">
#1 Generate config files
solrctl  --zk host1:2181,host2:2181/solr instancedir --generate myconf_local
</pre>

<pre class="prettyprint sh">
#2 Upload the new config files to Zookeeper
solrctl  --zk host1:2181,host2:2181/solr instancedir --create myconf myconf_local
</pre>

<pre class="prettyprint sh">
#3 List the new configuration
solrctl  --zk host1:2181,host2:2181/solr instancedir --list
</pre>

<pre class="prettyprint sh">
#4 Create a Solr collection with 2 shards
solrctl  --zk host1:2181,host2:2181/solr collection --create mycoll -s 2 
</pre>

<pre class="prettyprint sh">
#5 List the new collection
solrctl  --zk host1:2181,host2:2181/solr collection --list
</pre>

<pre class="prettyprint sh">
#6 Query the empty collection
curl http://localhost:8983/solr/mycoll/select?q=*:*
</pre>
