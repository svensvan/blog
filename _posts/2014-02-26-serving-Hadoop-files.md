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
			def url = 
				URLDecoder.decode(request.queryParams.url, "UTF-8").trim()

			if (url) {
				def har = new HarFileSystem()
				har.initialize(URI.create(url), new Configuration())
				def bos = new ByteArrayOutputStream()
				def in = null
				def ok = true
				try {
					in = har.open(new Path(url))
					IOUtils.copyBytes(in, bos, 4096, false)
	 			} 
	 			catch (Exception ex) {
	 				ok = false
		 		} finally {
	 				IOUtils.closeStream(in)
	 				har.close()
	 			}
	 			if (ok) {
		 			response.send(bos.toByteArray())
		 		} else {
		 			response.status(404)
		 			response.send()
			 	}
			}
		}
	}	
}


</pre>

The code below allows you to send requests

<pre class="prettyprint Java">

def client = new DefaultHttpClient()
def hget = new HttpGet(fileserver +"?url=" + URLEncoder.encode(url, "UTF-8"))
def resp = client.execute(hget)
if (resp.getStatusLine().getSTatusCode() == 200) {
	println resp.getEntity().getContent()	
}

</pre>
