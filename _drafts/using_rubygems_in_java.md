---
title: Using Ruby gems in Java
layout: post
author: Stefan
published: false
---

<b>Dependencies Section in pom.xml</b>

{% highlight xml %}
...
<!-- The Jruby Dependency -->
<dependency>
    <groupId>org.jruby</groupId>
    <artifactId>jruby</artifactId>
    <version>1.7.12</version>
</dependency>

<!-- a jar with the gem bundled  -->
<dependency>
    <groupId>org.nanoko.libs</groupId>
    <artifactId>compass-gems</artifactId>
    <version>0.12.2</version>
</dependency>
...
{% endhighlight %}


<b>JRubyRunner.java</b>

{% highlight java %}

import org.jruby.Ruby;
import org.jruby.javasupport.JavaEmbedUtils;

import java.util.ArrayList;
import java.util.List;

public class JRubyRunner {
    public static void main(String[] args) {
        List paths = new ArrayList();
        paths.add("classpath:gems/compass-0.12.2/lib");
        Ruby ruby = JavaEmbedUtils.initialize(paths);
        ruby.getLoadService().require("compass");
    }
}
{% endhighlight %}