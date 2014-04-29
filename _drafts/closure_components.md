---
title: Google Closure Components
layout: post
author: Stefan
published: false
---

In google closure, understanding the <code>goog.ui.Component</code> lifecycle
is a important part of understanding the library. The component provides a
interface to manage all the aspects of a given element.

### Components are **disposable**

This means that it can and must be disposed of when you are done using it.

{% highlight javascript %}
   // create component;
   var component = new my.Component();

   // do things with component
   component.render();

   // when done with component:
   component.dispose(); // destroys the component, all event listeners and other
                        // disposable objects attached to the component
{% endhighlight %}

### The <b>render</b> and <b>decorate</b> methods

A component can be created in two ways.

* The element already exists and we just decorate the component's behaviour onto it.
* The element does not exist and we tell the component to create it.

{% highlight javascript %}

   // the element exists in the document.
   var buttonElement = goog.dom.getElementByClass(goog.getCssName('my-button-element'));
   var button = new goog.ui.Button();
   if(goog.dom.isElement(buttonElement)){
     buttonElement.decorate();
   }

   // we ask the button to render its own element into the dom.
   var button = new goog.ui.Button();
   button.render();
{% endhighlight %}

### The <b>createDom</b> and <b>decorateInternal</b> methods


{% highlight javascript %}
  /**
    * @constructor
    * @extends {goog.ui.Component}
    */
   my.Component = function () {
      goog.base(this);
   };
   goog.inherits(my.Component, goog.ui.Component);

  /**
   * Creates a element like this
   * <div>
   *     <span>Title</span>
   * </div>
   * @override
   */
   my.Component.prototype.createDom = function () {
     var domHelper = this.getDomHelper();
     var element = domHelper.createDom('div');
     var titleElement = domHelper.createDom('span');
     titleElement.innerHTML = "Title";
     element.appendChild(titleElement);
     this.setElementInternal(element);
   };

   /**
    * @Override
    */
    my.Component.prototype.decorateInternal = function (element) {
      this.setElementInternal(element);
    };
{% endhighlight %}


### The <b>enterDocument</b> and <b>exitDocument</b> methods.

When a component enters the document, enterDocument is called and when it leaves exitDocument is called, this is where enable/disable any behaviour code.

Let us consider a example where you have a component with composes a button which it needs to decorate and listen to.


<p class="alert"><b>Warning:</b> Don't forget the goog.base calls when overriding enter and exitDocument</p>
{% highlight javascript %}

 /**
  * @constructor
  * @extends {goog.ui.Component}
  */
 my.Component = function () {
    goog.base(this);

    /** @type {goog.ui.Button} */
    this.button = new goog.ui.Button();
    this.registerDisposable(this.button);
 };
 goog.inherits(my.Component, goog.ui.Component);


  /**
   * @override
   */
  my.Component.prototype.enterDocument = function () {
    goog.base(this, 'enterDocument');

    /** @type {goog.events.EventHandler} */
    this.lifecycle = new goog.events.EventHandler(this);
    this.registerDisposable(this.lifecycle);
    this.lifecycle.listen(this.button, goog.ui.Components.EventType.ACTION, this.onButtonAction);

    /** @type {Element} */
    var buttonElement = this.getElementByClass(goog.getCssName("my-button"));
    if(goog.dom.isElement(buttonElement)){
      this.button.decorate(buttonElement);
    }
  };

  /**
   * @override
   */
  my.Component.prototype.exitDocument = function () {

    this.button.exitDocument();

    if(goog.isDefAndNotNull(this.lifecycle)) {
      this.lifecycle.dispose();
    }
    goog.base(this, 'exitDocument');
  };


  /**
   * @param {goog.events.Event} event
   */
  my.Component.prototype.onButtonAction = function (event) {
     alert("Button Clicked");
  };

{% endhighlight %}



### A complete example component.

{% highlight javascript %}

  goog.provide('my.Component');
  goog.require('goog.ui.Component');

  /**
   * @constructor
   */
  my.Component = function () {
    goog.base(this);
  };
  goog.inherits(my.Component, goog.ui.Component);

  /**
   * @override
   */
  my.Component.prototype.enterDocument = function () {

  };

  /**
   * @override
   */
  my.Component.prototype.exitDocument = function () {

  };


  /**
   * @override
   */
  my.Component.prototype.createDom = function () {

  };



{% endhighlight %}