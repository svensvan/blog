---
layout: post
title: "Serving Hadoop Files"
---

Hadoop is great for storing and retrieving files. Here is one way to serve up files that are
	stored in a Hadoop archive. Warning: no securing is mentioned in this post.  You'll need to implement your own

The code below demonstrates how easy it is to use Ratpack (a wrapper around Netty) to make an app server.

<pre class="prettyprint Java">
@Grapes([
	@GrabResolver("https://oss.jfrog.org/artifactory/repo"),
	@Grab("io.ratpack:ratpack-groovy:0.9.0-SNAPSHOT"),
	@Grab("org.apache.hadoop:hadoop-client:2.0.0-alpha"),
	@GrabExclude('com.sun.jdmk:jmxtools'),
	@GrabExclude('com.sun.jmx:jmxri'),
])

import static ratpack.groovy.Groovy.*

import org.apache.hadoop.fs.*
import org.apache.hadoop.conf.*
import org.apache.hadoop.io.*
import org.apache.hadoop.mapred.*
import org.apache.hadoop.util.*

import java.net.*

ratpack {
	handlers {
		get("file") {
			def url = request.queryParams.url

			if (url) {
				def c = new Configuration()
				def fs = org.apache.hadoop.fs.
					FileSystem.get(URI.create(url), conf)
				def bos = new ByteArrayOutputStream()
				def in = null
				try {
					in = fs.open(new Path(url))
					IOUtils.copyBytes(in, bos, 4096, false)
	 			} 
	 			catch (Exception ex) {}
	 			finally { IOUtils.closeStream(in)}
	 			response.send(bos.toByteArray())

			}
		}
	}	
}


</pre>

