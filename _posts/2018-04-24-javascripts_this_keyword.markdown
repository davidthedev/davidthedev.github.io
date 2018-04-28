---
layout: post
title:  "Deep dive into this keyword in Javascript"
date:   2018-04-24 21:00:00 +0000
categories: javascript closures
---

<code>this</code> is an interesting, at the same time confusing mechanism in JavaScript. These is nothing really advanced about it, however it is a bit of a mystery for some developers so we will try to demystify it together in this post.

If you are coming from an OOP language such as PHP or Java, then <code>this</code> simply refers the current object. In JavaSript however it is much more than that.

Every time a function is invoked, a new execution context is created and <code>this</code> comes for free with it. The value of <code>this</code> is dependent on the conditions under which the function was created.

<h3>Global scope</h3>

In JavaScript <code>this</code> keyword is available at the global level, so if you run <code>console.log(this)</code> you will get the <a href="https://developer.mozilla.org/en-US/docs/Web/API/Window">window object</a> back. In the examples below we will be looking at how JavaScript's <code>this</code> keyword generally behaves.

{% highlight javascript %}

function makeNoise() {
  console.log(this);
}

makeNoise();

{% endhighlight %}

In the above example, we are defining and invoking <code>makeNoise()</code> function. Whenever we invoke a function at the global level, <code>this</code> will point to the window object. This way you can also add properties and methods to the window object, and retrieve them elsewhere in the code.

Let's take a look at another example.

{% highlight javascript %}

const make = 'VW';
const model = 'Golf';

function getCar() {
  return this.make + ' ' + this.model;
}

console.log(getCar());

{% endhighlight %}

As we already know, when using <code>this</code> at the global scope, it refers to the window object. In the our example we are defining two constants <code>make</code>, <code>model</code> and a function <code>getCar()</code> globally. By running the function <code>getCar()</code> and using <code>this</code> inside of it, we are accessing data that is defined in the window object. The output of this function will be 'VW Golf'.

<i>Gotcha</i>

Note that if you are using strict mode then <code>this</code> will be set to <b>undefined</b>.

<h3>Object</h3>

Let's have a look at the example where we are using <code>this</code> inside of an object.

{% highlight javascript %}

const make = 'BMW';
const model = '318';

const car = {
  make: 'VW',
  model: 'Golf',
  getCar: function() {
    return this.make + ' ' + this.model
  }
}

console.log(car.getCar());

{% endhighlight %}

When using <code>this</code> operator inside of an object we are refering to the same object within which <code>this</code> was called. Essentialy, <code>this</code> is a referent to the object in context. When we call method <code>getCar()</code>, we create a new execution context and JavaScript works out what <code>this</code> should be referring / pointing to. In our example we are referring to the <code>car</code> object and its properties. Note that we also defined two constants with the same names as the object properties. This is done prove that we are not accessing the global scope. We should expect the output to be 'VW Golf'.

Something else to note here is that we could also access the properties from the <code>getCar()</code> method using <code>car.make</code> and <code>car.model</code>. However this is a very vague way of accessing properties, as there might be other objects called <b>car</b> that we might not be aware off and this could mess things up. It is recommended to use <code>this</code> to let JavaScript know that we want to access this exact object.

<h3>Closures</h3>

{% highlight javascript %}

const carExample = {
  makeModel: {
    make: 'VW',
    model: 'Golf'
  },
  getCar: function() {
    return function() {
      return this.makeModel.make + ' ' + this.makeModel.model;
    }
  }
}

const car = carExample.getCar();
console.log(car());

{% endhighlight %}

TODO
