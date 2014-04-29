---
layout: post
title: Soy Annotations
author: Stefan
published: false
---

<p class="notice">
  <b>Disclaimer:</b> I wrote Soy Annotations.
</p>

So, [Soy Annotations][1] is a library that provides additional features to translate objects into soyData.


## Bootstrapping the Injector

SoyAnnotations rely on Guice injections, so you will need to create a injector that will instantiate the classes.

There are three flavours to bootstrap, which I will cover later. For now we will use the Full Injector.

{% highlight java %}
  Injector injector = Loader.getFullInjector();
{% endhighlight %}

## Creating the context

{% highlight java %}
  SoyDataFactoryContext context = injector.getInstance(SoyDataFactoryContext.class);
{% endhighlight %}

## Creating Converters for Basic Types

SoyAnnotations uses reflection to build a converters for each Class it encounters, it caches those converters classes for efficiency.

{% highlight java %}
  Converter<Object, ? extends SoyData> converter = context.create(String.class);
  SoyData soyData = converter.convert("some string"); // returns a StringData instance of SoyData
{% endhighlight %}

| Annotation                           | Target | Description                                                                                |
|:-------------------------------------|:------:|:-------------------------------------------------------------------------------------------|
| `@Soy(useOriginalToString = false)`  | Type   | The Soy annotation indicates a class that can be converted into soy data.                  |
| `@Soy.Method(String)`                | Method | Specifies that the method's result is can be used as a property on the resulting soy data. |
| `@Soy.Field(String)`                 | Field  | Specifies that the field can be used as a property on the resulting SoyData                |
| `@Soy.Template(String)`              | Type   | Specifies the template that is used when rendering the class.                              |


## Annotating Classes

{% highlight java %}
  @Soy
  public class User {
    private String name;

    public User(String name) {
      this.name = name;
    }

    @Soy.Method("Name")
    public String getName() {
      return name;
    }
  }
{% endhighlight %}

## Rendering Objects

{% highlight java %}
  RendererFactoryContext context = injector.getInstance(RendererFactoryContext.class);
    context.render(new User());
{% endhighlight %}

## Adding Custom Factories

<p class="alert"><b>Missing:</b> Section on adding factories to injection.</p>


## Adding Custom Converters

{% highlight java %}
  /**
   * This converter turns users into a greeting string
   */
  public class GreeterUserConverter implements Converter<User, String> {
     @Override
     public String convert(User user) {
       return String.format("Hello %s, welcome", user.getName());
     }
  }
{% endhighlight %}


[1]:http://github.com/StefanLiebenberg/SoyAnnotations