---
layout: post
title: "The fatjar for Storm using Gradle"
---

Storm is pretty good for building a realtime processing engine. But before you get started, you need a build file that creates a fatjar. This is the gradle build file for running my test Groovy script and also building my fatjar.

<pre class="prettyprint Groovy">
apply plugin: 'groovy'
apply plugin: 'java'
apply plugin: 'application'

configurations {
	keepOutOfJar
	compile.extendsFrom keepOutOfJar	
}

dependencies {
	keepOutOfJar('storm:storm:0.9.0.1')	
	compile('other jars')
	if (System.getProperty("ENVRIONMENT")?.equals("PRODUCTION")) {
		compile localGroovy()	
	}
}

task fatJar(dependsOn: [classes], type:Jar) {
	dependsOn configurations.runtime
	from files(sourceSets.main.output.classesDir)
	from files(sourceSets.main.output.resourcesDir)
	from {
		(configurations.runtime - configurations.keepOutOfJar).collect {
			it.isDirectory() ? it : zipTree(it)
		}
	}
	manifest {
		attributes 'Main-Class': 'MainClass'
	}
}

task runGroovy(dependsOn: [classes], type: JavaExec) {
	main = 'MainClass'
	classpath = sourceSets.main.runtime.Classpath
}
</pre>

