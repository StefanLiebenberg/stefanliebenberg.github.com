---
title: Maintain an Maven Central project on Github.
layout: post
author: Stefan
---


<p class="notice">This document is a Draft</p>

Steps:

 * [Create a GitHub Repo][1]
 * Create project on Sonatype
 * Setup travis
 * To deploy snapshot
 * To deploy release


## Clone Project locally

{% highlight bash %}
  $ git clone <github-project-url>
{% endhighlight %}

## Create a pom file and maven structure.

{% highlight bash %}
  $ cd <project-name>
  $ touch pom.xml
  $ mkdir -p src/{main,test}/java
{% endhighlight %}

## Setup pom file

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.company</groupId>
    <artifactId>project</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>jar</packaging>
    <name>Project Name</name>
    <description>Project Description</description>
    <url>http://github.com/<GitHubUser>/<Project></url>

    <licenses>
        <license>
            <name>MIT License</name>
            <url>http://www.opensource.org/licenses/mit-license.php</url>
            <distribution>repo</distribution>
        </license>
    </licenses>


    <developers>
        <developer>
            <id>user@company.com</id>
            <email>user@company.com</email>
            <name>User Name</name>
            <url>http://github.com/User</url>
        </developer>
    </developers>

    <parent>
        <groupId>org.sonatype.oss</groupId>
        <artifactId>oss-parent</artifactId>
        <version>9</version>
    </parent>

    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.1</version>
                <configuration>
                    <source>1.7</source>
                    <target>1.7</target>
                    <encoding>UTF-8</encoding>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-source-plugin</artifactId>
                <version>2.2.1</version>
                <executions>
                    <execution>
                        <id>attach-sources</id>
                        <goals>
                            <goal>jar</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-javadoc-plugin</artifactId>
                <version>2.9.1</version>
                <executions>
                    <execution>
                        <id>attach-javadocs</id>
                        <goals>
                            <goal>jar</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>

    <dependencies>
        ...
    </dependencies>

    <scm>
        <connection>
            scm:git:git://github.com/User/Project.git
        </connection>
        <developerConnection>
            scm:git:git@github.com:User/Project.git
        </developerConnection>
        <url>http://github.com//User/Project</url>
        <tag>HEAD</tag>
    </scm>
</project>
{% endhighlight %}



[1]:https://help.github.com/articles/create-a-repo



