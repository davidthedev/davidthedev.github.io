---
layout: post
title:  "Deep dive into this keyword in Javascript"
date:   2018-04-24 21:00:00 +0000
categories: javascript closures
---

<code>this</code> is an interesting at the same time confusing mechanism in JavaScript. These is nothing really advanced about it, however it is a mystery for many developers and we will try to demystify it together in this post.

If you are coming from a PHP background, then <code>this</code> simply refers an instance of the object. In Javascript however it is much more than that.


Every time a function is run / invoked, a new execution context is created and <code>this</code> comes for free with it. Essentially, <code>this</code> is a referent to the object in context. The value of this is dependant on how a function is called (in what context). <code>this</code> keyword is available at the global level, so if you run <code>console.log(this)</code> you will get the window object.

{% highlight javascript %}

function makeNoise() {
  console.log(this);
}

makeNoise();

{% endhighlight %}

We are defining <code>makeNoise()</code> function and then invoking it. <code>this</code> will still point to the global object. So whenever we are creating a function at the global level, <code>this</code> will be pointing to the global / window object, the same address in the memory. This way you can also assign variables to the global object and retrieve them elsewhere.

<b>- Global scope</b>

In the example below we are looking at how JavaScript's this keyword behaves in the global scope.

{% highlight javascript %}

const make = 'VW';
const model = 'Golf';

function getCar() {
  return this.make + ' ' + this.model;
}

console.log(getCar());

{% endhighlight %}

When using <code>this</code> in the global scope, it will refer to the window object. In the our example we are defining two constants <code>make</code>, <code>model</code> and a function <code>getCar()</code> globally. By running the function and using <code>this</code> inside of it, we are accessing data that is defined in the window object. The output for this will be 'VW Golf'.

<i>Gotcha</i>

In script mode this is undefined - TODO

<b>- Object</b>

TODO

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

When using <code>this</code> operator inside of an object we are refering to the same object within which the <code>this</code> was called. When we call method <code>getCar()</code>, we create a new execution context and JavaScript works out what <code>this</code> should be referring / pointing to. In our example we are referring to the <code>car</code> object and its properties. Note that we also defined two constant with the same names as the object properties to prove that we are not accessing the global scope. We should expect the output to be 'VW Golf'. Something else to note here is that we could also access the properties from the <code>getCar()</code> method using <code>car.make</code> and <code>car.model</code>. However this is a very vague way of accessing object properties, as there might be other objects called car object that we might not be aware off and this could mess things up. It is recommended to use <code>this</code> to let JavaScript know that we want to access '<code>this</code> exact object'

<b>- Closures</b>

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


<code>this</code> is an interesting at the same time confusing keyword in JavaScript. Every time a function is run / invoked, a new execution context is created and <code>this</code> comes for free with it.

<code>this</code> is available at the global level, so if you run <code>console.log(this)</code> you will get the window object. In this example this points to the global scope.

