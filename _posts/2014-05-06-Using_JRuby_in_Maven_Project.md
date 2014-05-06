---
title: Using JRuby in Maven Project
layout: post
author: Stefan
published: true
---

Integrating a ruby environment into a Java maven project is quite easy. I recently built [BlenderCss][1], a java project that needed to use the [Compass][2] gem. To solution was to utilize the gem via jruby.


### You will need..

##### 1. Dependencies Section in pom.xml

{% highlight xml %}
...
<!-- The Jruby Dependency -->
<dependency>
    <groupId>org.jruby</groupId>
    <artifactId>jruby</artifactId>
    <version>1.7.12</version>
</dependency>
...
{% endhighlight %}

#### 2. The ruby class you plan to use.


{% highlight ruby %}
class MyApi

end
{% endhighlight %}


<b>JRubyRunner.java</b>

{% highlight java linenos %}

import org.jruby.Ruby;
import org.jruby.javasupport.JavaEmbedUtils;

import java.util.ArrayList;
import java.util.List;

public class JRubyRunner {
    public static void main(String[] args) {
        List paths = new ArrayList();
        paths.add("classpath:gems/compass-0.12.2/lib");
        Ruby ruby = JavaEmbedUtils.initialize(paths);
        ruby.getLoadService().require("compass");  // require "compass"
        ruby.evalScriptlet("Compass::Exec::SubCommandUI.perform! :compile"); // perform a compile command.
    }
}
{% endhighlight %}




<p class="info">
  <b>Note:</b> jRuby is slow to initialize, so use it wisely.
</p>


[1]:http://github.com/StefanLiebenberg/BlenderCss
[2]:http://compass-style.org/