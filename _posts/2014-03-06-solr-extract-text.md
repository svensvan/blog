---
layout: post
title: "Setting up SolrCloud to Extract Text"
---

First, put the librariies in a folder accessible by all servers.  You will need the contrib/extraction libraries found
in the regular Solr distribution, as well as the solr-cell* one found in SolrCloud.

Second, you'll need to update the lib tag in solrconfig to point to the right folder.

Finally, you'll need to add the following request handler to solrconfig:

<pre class="prettyprint xml">
&lt;requestHandler name=“/update/extract” class=“org.apache.solr.handler.extraction.ExtractingRequestHandler”&gt;
  &lt;lst name=“default”&gt;
    &lt;str name=“uprefix”&gt;ignored_&lt;/str&gt;
  &lt;/lst&gt;
&lt;/requestHandler&gt;
</pre>

Test the above by going to /update/extract using params extractOnly=true and extractFormat=text -F "thefile=@file"
