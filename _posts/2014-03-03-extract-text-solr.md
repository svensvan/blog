---
layout: post
title: "Using SolrJ Client to Extract Text"
---

Solr has a module that extracts text from binary files.  One option is just send the bytes up to Solr and have
Solr return the extracted content along with some useful metadata.

<pre class="prettyprint Java">
	def request = new ContentStreamUpdateRequest("/update/extract")
	request.with {
		addContentStream(new ContentStreamBase.ByteArrayStream(bytes, "file"))
		setParam "extractOnly", "true"
		setParam "extractFormat", "text"
	}
	doc = server.request(request)
	doc[1].value //text
	doc[2].value.get("title")
	doc[2].value.get("meta:author")
	doc[2].value.get("meta:creation-date")

</pre>

