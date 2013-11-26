---
layout: post
title: "Groovy Jenkins"
date: 2012-03-29 22:34
comments: true
categories: [jenkins,groovy]
---

 For some reason the Gradle instructions for Jenkins plugins did not work for me. 
 I resorted on doing something like this in build.gradle:

``` 
buildscript {
repositories {
    mavenLocal()
    mavenCentral()
        maven {
            name "jenkins"
                delegate.url("http://maven.jenkins-ci.org/content/repositories/releases/")
        }
    // The plugin is currently only available via the Jenkins Maven repository.
}
dependencies {
    classpath group: 'org.jenkins-ci.tools', name: 'gradle-jpi-plugin', version: '0.3.3'
}
}

apply plugin: 'jpi'

dependencies {
groovy localGroovy()
}

groupId = "org.jenkins-ci.plugins"
version = "0.0.1-SNAPSHOT"    // Or whatever your version is.
description = "A description of your plugin"

jenkinsPlugin {
coreVersion = '1.420'                                               // Version of Jenkins core this plugin depends on.
    displayName = 'Plugin for Teambox integration'                // Human-readable name of plugin.
    url = ''   // URL for plugin on Jenkins wiki or elsewhere.

    // The developers section is optional, and corresponds to the POM developers section.
    developers {
        developer {
            id 'heimojuh'
                name 'Juha Heimonen'
                email 'heimojuh@gmail.com'
        }
    }                           

}
```

And after that I noticed: oh yeah, <b> mavenCentral() </b>. The upper excerpt is missing from many examples, and as basic as it may be, I at least have hard time remembering it. I wonder if it should be in https://raw.github.com/jenkinsci/gradle-jpi-plugin/master/install or are these easily embeddable to build script in parent? Well, beats me. At least this seems to work. And it's rather more pleasant than Maven and pom if you ask me. 
