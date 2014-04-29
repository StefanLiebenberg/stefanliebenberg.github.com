---
title: Soy Annotations
layout: post
---

<p class="alert">This is a draft document</p>

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

| Annotation                                | Example                    | Description                                                                |
|-------------------------------------------|----------------------------|----------------------------------------------------------------------------|
| `@Soy(useOriginalToString = false)`       |                            |  The Soy annotation indicates a class that can be converted into soy data. |
|                                           |                            |   |
| `@Soy.Method(String)`                |                            |  The Soy.Method annotation            |
| `@Soy.Field(String)`                 |                            |  The Soy.Method annotation            |
| `@Soy.Template(String)`                 |                            |  The Soy.Method annotation            |


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

## Adding Custom Factories

## Adding Custom Converters

[1]:http://github.com/StefanLiebenberg/SoyAnnotations