---
title: The lifecycle Event Handler in Google Closure Components.
layout: post
author: Stefan
---

<p>
Managing events in the lifecycle of a component can be tricky, the standard approach is to use the getHandler() and to listen and unlisten to each class.
</p>

{% highlight javascript %}
/** @override */
my.Component.prototype.enterDocument = function () {
  goog.base(this, 'enterDocument');
  var element = this.getElementStrict();
  var handler = this.getHandler();
  handler.listen(element, goog.events.EventType.CLICK, this.onElementClick);
};

/** @override */
my.Component.prototype.exitDocument = function () {
  var element = this.getElementStrict();
  var handler = this.getHandler();
  handler.unlisten(element, goog.events.EventType.CLICK, this.onElementClick);
  goog.base(this, 'exitDocument');
};
{% endhighlight %}

<p>
There is a cleaner approach to this, the handler is a disposable object, so we can easily create one just for the time the component is displayed on screen. When the component leaves the screen, disposing this handler will ensure that all event listeners are remove correctly.
</p>



{% highlight javascript %}
/** @override */
my.Component.prototype.enterDocument = function () {
  goog.base(this, 'enterDocument');

  var element = this.getElementStrict();

  /** @type {goog.events.EventHandler} */
  this.lifecycle = new goog.events.EventHandler(this);
  this.registerDisposable(this.lifecycle);

  this.lifecycle.listen(element, goog.events.EventType.CLICK, this.onElementClick);
};

/** @override */
my.Component.prototype.exitDocument = function () {
  if(goog.isDefAndNotNull(this.lifecycle)){
    this.lifecycle.dispose();
  }
  goog.base(this, 'exitDocument');
};
{% endhighlight %}