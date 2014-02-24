---
layout: post
title: "Archiving files in Hadoop"
---

<p>Hadoop is great for storing and retrieving files. Here are some steps
for storing and retreiving files in Hadoop</p>

<p>The script below will create a Hadoop archive from files stored on your local directory</p>

<pre class="prettyprint lang-sh">
#!/bin/sh
hadoop archive -archiveName myarchive.har -p /User/directory hdfs:(namenode):8020/
</pre>

<p><br>You may find that Hadoop has a problem with some of your files.  It doesn't like file names with spaces, or special characters like [,( or %.  Here is a script that will clean up those files:</p>

<pre class="prettyprint lang-sh">
#!/bin/sh
find (directory) -depth -name "* *" -execdir bash -c 'mv "$1" "${1// /_}"' _ {} \;
</pre>

<p><br>If you want to view the files in your archive run this command:</p>

<pre class="prettyprint lang-sh">
#!/bin/sh
hadoop fs -ls -R har:(namenode):8020/myarchive.har | grep -v ^d | grep -o har://.*
</pre>
