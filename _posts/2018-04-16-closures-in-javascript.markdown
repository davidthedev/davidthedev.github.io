---
layout: post
title:  "Closures in JavaScript"
date:   2018-04-16 21:00:00 +0000
categories: javascript closures
---

In JavaScript closures are everywhere. They enable you to write more expressive and better code. You may not even realise this but you are already using closures in your code. At first, the word closure sounds quite intimidating, however once you understand what it is and how to use it, it becomes second nature.

If you want to become a developer who uses advanced concepts and techniques, aces your next job interview, gets a great job offer (therefore potentially pockets large sums of cash :))) ) then you simply cannot afford to ignore and must master closures.

Now, it's not all about the money, it's about the knowledge, so let's dive right in.

There are many definitions online of what a closure is. First let's recap, every time a JavaScript function is created, a new scope is created with it and all of its data is only available inside that scope. There are 3 different scope levels, a function can access its own scope, a function can access the outer scope and global scope. A closure in JavaScript is a function that has access to the scope it was created outside of (outer scope). In other words, the function remembers where it was created (the environment it was created in) and keeps a reference to it. One important thing to note here is that the access to the outer environment is by reference, not by value (we'll come back to this later). Something else to remember is that when a function is created in JavaScript it technically becomes a closure.

Let’s have a look at a quick example:

{% highlight javascript %}

function outer() {
  var outerEnvironment = 'outer';

  function inner() {
    return 'I have access to ' + outerEnvironment + ' environment';
  }

  return inner;
}

var outerFunc = outer(); // we are storing function declaration
outerFunc(); // we are invoking the function

{% endhighlight %}

Take some time to go through the code. What do you expect to happen here? The function <code>inner()</code> has (lexical scope) access to its outer environment (scope), it closes over the scope of <code>outer()</code>. Note that we are not invoking <code>inner()</code> but returning its function object (all functions are objects in JavaScript, remember?). We are then storing the function object in a variable and invoking it on the next line. The output in the console should be: <i>'I have access to outer environment'</i>.

<h3>Usage / gotchas</h3>

<b>- Encapsulation</b>

One of the practical uses of closures in JavaScript is data encapsulation (privacy). You associate data with function(s) that operate(s) on that data. If we compare this to object-oriented programming then data is the object's property, function is a method. Essentially with closures we can have many functions and variables inside the function but only expose what we need to by creating a public facing API. This way everything else other than the exposed API will be kept private. This is known as the module pattern. A module in JavaScript is just an organised, self-contained portion of code. And since JavaScript natively does not support classes the module pattern solves this.

Let's have a look at another example:

{% highlight javascript %}

function bankAccount() {
  var balance = 0;

  function updateAccount(operation, amount) {
    switch(operation) {
      case 'deposit':
        balance += amount;
        break;
      case 'take-out':
        balance -= amount;
        break;
      default:
        break;
    }
  }

  return {
    deposit: function(amount) {
      updateAccount('deposit', amount);
    },
    takeOut: function(amount) {
      updateAccount('take-out', amount);
    },
    getBalance: function() {
      console.log(balance);
    }
  };
}

var account = bankAccount();
account.deposit(10);
account.getBalance();

{% endhighlight %}

In this short example we are emulating a very simple bank account. We have a function <code>bankAccount()</code>, inside of this function (in its scope) we are storing a variable <code>balance</code> and a function that updates the balance - <code>updateAccount()</code>. Finally, we are returning an object which allows to add, subtract and retrieve the balance. Essentially, only the desired functionality is being exposed this way. When you run this code, you should see the account balance - <code>10</code>. If we try accessing <code>updateAccount()</code> we'll get an error. Why you might wonder? Well, we are accessing the parent scope from the object that we are returning but protecting the guts of <code>updateAccount()</code>. The three functions that we are returning are closures and have access to <code>updateAccount</code> method due to the lexical scoping. There is also a single lexical environment which is shared by these functions. This way we are able to only expose we want and protect the rest.

<b>- Loops</b>

I mentioned earlier that a closure has access to outer scope by the reference. This can create an unexpected behaviour in JavaScript. Let's have a look at an example:

{% highlight javascript %}

function here() {
  var j = 10;

  for (i = 0; i < 5; i++) {
    test = function() {
      console.log(j + ' ' + i);
    }
  }

  return test;
}

var check = here()

check()

{% endhighlight %}

Run this code and you will be surprised. When I first came across this, I definitely was. So what happens here is that our closure gets the value of <code>i</code> only after the entire loop runs. To get this working the way we expect it to work (to get the value of <code>i</code> on each iteration) we need to use the Immediately Invoked Function Expression (IIFE), a function that gets invoked as soon as it is defined.

{% highlight javascript %}

function here() {
  var j = 10;

  for (i = 0; i < 5; i++) {
    test = function() {
      console.log(j + ' ' + i);
    }(i)
  }

  return test;
}

var check = here()

check()

{% endhighlight %}

Now we are immediately invoking the function and passing the value of <code>i</code> into it.

That's it for this post and I hope that all of the above made sense and wasn't too complicated. I will try to update this articles as I come across more examples.

Further reading suggestions:

<a href="https://github.com/getify/You-Dont-Know-JS/blob/master/scope%20%26%20closures/ch5.md">You don't know JS - Closures</a>

<a href="https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-closure-b2f0d2152b36">Master the JavaScript Interview: What is a Closure?</a>

<a href="http://javascriptissexy.com/understand-javascript-closures-with-ease">Understand JavaScript Closures With Ease</a>
