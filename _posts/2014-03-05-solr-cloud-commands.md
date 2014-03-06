---
layout: post
title: "Getting started with SolrCloud"
---

The steps below will create a new Solr collection. 

1. Generate the config files
	<pre class="prettyprint sh">
	solrctl  --zk host1:2181,host2:2181/solr instancedir --generate myconf_local
	</pre>

2. Upload the new config files to Zookeeper
	<pre class="prettyprint sh">
	solrctl  --zk host1:2181,host2:2181/solr instancedir --create myconf myconf_local
	</pre>

3. List the new configuration
	<pre class="prettyprint sh">
	solrctl  --zk host1:2181,host2:2181/solr instancedir --list
	</pre>

4. Create a Solr collection with 2 shards
	<pre class="prettyprint sh">
	solrctl  --zk host1:2181,host2:2181/solr collection --create mycoll -s 2 
	</pre>

5. List the new collection
	<pre class="prettyprint sh">
	solrctl  --zk host1:2181,host2:2181/solr collection --list
	</pre>

6. Query the empty collection
	<pre class="prettyprint sh">
	curl http://localhost:8983/solr/mycoll/select?q=*:*
	</pre>
