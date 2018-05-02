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

In JavaScript <code>this</code> keyword is available at the global level, so if you run <code>console.log(this)</code> in a web browser you will get the <a href="https://developer.mozilla.org/en-US/docs/Web/API/Window">window object</a> back. In the examples below we will be looking at how JavaScript's <code>this</code> keyword generally behaves.

{% highlight javascript %}

function makeNoise() {
  console.log(this);
}

makeNoise();

{% endhighlight %}

In the this example, we are defining and invoking <code>makeNoise()</code> function. Whenever we invoke a function at the global level, <code>this</code> will point to the window object. This way you can also add properties and methods to the window object, and retrieve them elsewhere in the code.

Let's take a look at another example.

{% highlight javascript %}

const make = 'VW';
const model = 'Golf';

function getCar() {
  return this.make + ' ' + this.model;
}

console.log(getCar());

{% endhighlight %}

As we already know, when using <code>this</code> at the global scope, it refers to the window object. In our example we are defining two variables <code>make</code>, <code>model</code> and a function <code>getCar()</code> globally. By running the function <code>getCar()</code> and using <code>this</code> inside of it, we are accessing data that is defined in the window object. The output of this function will be 'VW Golf'.

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

When using <code>this</code> operator inside of an object we are refering to the same object within which <code>this</code> was called. Essentialy, <code>this</code> is a referent to the object in context. When we call method <code>getCar()</code>, we create a new execution context and JavaScript works out what <code>this</code> should be referring / pointing to. In our example we are referring to the <code>car</code> object and its properties. Note that we also defined two variables with the same names as the object properties. This is done to prove that we are not accessing the global scope. We should expect the output to be 'VW Golf'.

Something else to note here is that we could also access the properties from the <code>getCar()</code> method using <code>car.make</code> and <code>car.model</code>. However this is a very vague way of accessing properties, as there might be other objects called <b>car</b> that we might not be aware off and this could mess things up. It is recommended to use <code>this</code> to let JavaScript know that we want to access this exact object.

<h3>Closures</h3>

{% highlight javascript %}

const car = {
  makeModel: {
    make: 'VW',
    model: 'Golf'
  },
  colour: {
    main: 'black'
  },
  getDetails: function() {
    return function() {
      return 'Make: ' + this.makeModel.make + ' Model: ' + this.makeModel.model + ' Colour: ' + this.colour.main;
    }
  }
}

const carInfo = car.getDetails();
console.log(carInfo());

{% endhighlight %}

This is an interesting one. We have an object literal <code>car</code> with two properties, one is an object and another one is a function that returns a function. This last function should return a concatenated string which gives us information about the car. However when we run the code we get <i>Uncaught TypeError: Cannot read property 'make' of undefined</i>. So what's going on here? Intuitively, one would think that <code>this</code> points to the <code>car</code> object. Do you remember we said at the beginning that when a function is created a new execution context is created as well? This is what is happening in our example. <code>this.makeModel.make</code> is pointing to the global object and looking for it there. But <code>makeModel</code> object does not exist in the global object hence we are getting the error. A way to solve this is to create a variable inside the first function and assign <code>this</code> to it.

{% highlight javascript %}

const car = {
  makeModel: {
    make: 'VW',
    model: 'Golf'
  },
  colour: {
    main: 'black'
  },
  getDetails: function() {
    const self = this;

    return function() {
      return 'Make: ' + self.makeModel.make + ' Model: ' + self.makeModel.model + ' Colour: ' + self.colour.main;
    }
  }
}

const carInfo = car.getDetails();
console.log(carInfo());

{% endhighlight %}

Objects are set by reference in JavaScript so we are essentially referencing the original <code>this</code>. Running the updated code should produce: 'Make: VW Model: Golf Colour: black'.

Let's have a look at one more example:

{% highlight javascript %}

const car = {
  makeModel: {
    make: 'VW',
    model: 'Golf'
  },
  colour: {
    main: 'black'
  },
  getDetails: function() {
    return 'Make: ' + this.makeModel.make + ' Model: ' + this.makeModel.model + ' Colour: ' + this.colour.main;
  }
}

const carInfo = car.getDetails;
console.log(carInfo());

{% endhighlight %}

Essentially, we are reassining a method <code>getDetails</code> to a variable and it simply becomes a function. Therefore, the concept of this is no loger valid and it does not refer to the <code>car</code> context anymore. It refers to the global object. We would get <code>undefined</code> in the output. What we can to do here is bind it to the <code>car</code> context.

{% highlight javascript %}

const car = {
  makeModel: {
    make: 'VW',
    model: 'Golf'
  },
  colour: {
    main: 'black'
  },
  getDetails: function() {
    return 'Make: ' + this.makeModel.make + ' Model: ' + this.makeModel.model + ' Colour: ' + this.colour.main;
  }
}

const carInfo = car.getDetails;
const carInfoBound = carInfo.bind(car);
console.log(carInfoBound());

{% endhighlight %}

Now <code>this</code> is bound to <code>car</code> and works how we would expect it to work.

Further reading suggestions:

<a href="https://github.com/getify/You-Dont-Know-JS/blob/master/this%20%26%20object%20prototypes/ch1.md">You don't know JS - this & Object Prototypes</a>

<a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this">MDN web docs - this</a>
