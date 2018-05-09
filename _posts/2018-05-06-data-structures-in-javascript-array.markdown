---
layout: post
title:  "Data Structures in JavaScript"
date:   2018-05-06 21:00:00 +0000
categories: javascript datastructures
---

<h3>Arrays</h3>

An array is a collection of a fixed number of elements of the same type (int, char, float, etc.) with each element having its own index. The elements can be accessed via indices which are usually integers. It is one of the oldest, simplest and most fundamental data structures. Arrays can be used on their own or often to implement other data structures. For example, a string is usually identified as a data type is often implemented as an array of characters. In JavaScript strings behave like read-only arrays of characters.

There are several types of arrays the simplest being a linear (one-dimensional) array. As a matter of the fact, all of the data structures in JavaScript are built on one cental abstract data structure - associative array, an advanced type of array. More on this latter.

Native arrays are fast. What is interesting however is that even though we call them arrays in JavaScript, internally they are special types of objects identified as arrays. Its interger indices are converted to strings to comply with the object format. The downside to this is that arrays in JavaScript are not as efficient as in some other programming languages that implement a native array data structure (e.g. Java).

The special array object comes with many methods (e.g. indexOf, pop, push, etc.) available to us. These methods are inherited from Array.prototype object that is used in the construction of arrays. Unlike other languages, JavaScript allows array elements to be of mixed data types.

{% highlight javascript %}

const coloursAndNumbers = [];
coloursAndNumbers.push('blue');
coloursAndNumbers.push('red');
coloursAndNumbers.push(2);

console.log(coloursAndNumbers.indexOf('blue')); // outputs 1
console.log(coloursAndNumbers.length); // outputs 3
console.log(coloursAndNumbers); // outputs   ['blue', 'red', 2]

{% endhighlight %}

<h3>Multidimensional arrays</h3>

Arrays in JavaScript are uninitialised and one-dimensional. If we want to have a multidimensional array, we will have to build one. In languages like Java we can create multi-dimensional arrays with the default value of each element being zero.

{% highlight java %}

import java.util.Arrays;

public class HelloWorld
{
  public static void main(String[] args)
  {
    int[][] someArray;
    someArray = new int[2][4];
    someArray[1][2] = 8; // assign 8 to the 3rd element in the 2nd array

    System.out.println(Arrays.deepToString(someArray)); // prints: [[0, 0, 0, 0], [0, 0, 8, 0]]
  }
}

{% endhighlight %}

As you see we can assign values to a particular element in a particular array with no problem at all. However, in JavaScript this is done a bit differently. We need to manually (i.e. there isn't a function for it) initialise and populate an array in JavaScript. When creating an array with <code>[]</code> notation, essentially we are creating just an completely empty array. But as we know by definition an array should be of a fixed length definitely before the array is created. However in JavaScript arrays are dynamic, meaning that they will resize when we need them to.

{% highlight javascript %}

Array.initialise = function(arraySize, defaultElementValue) {
  let newArray = [];

  for (let i = 0; i < arraySize; i++) {
    newArray[i] = defaultElementValue;
  }

  return newArray;
}

const test = Array.initialise(10, 0);
console.log(test); // outputs [0, 0, 0, 0, 0, 0, 0, 0, 0, 0]

{% endhighlight %}

This works however there is a serious flaw with this. If we pass in an empty array as an initial value of each element, we would get a reference to the same array. We must explicitely set each element.

{% highlight javascript %}

Array.initialise = function(rowLength, arraySize, defaultElementValue) {
  let arrayColumn = [];

  for (let i = 0; i < rowLength; i++) {
    let arrayRow = [];

    for (let x = 0; x < arraySize; x++) {
      arrayRow[x] = defaultElementValue;
    }
    arrayColumn[i] = arrayRow;
  }

  return arrayColumn;
}

const myMatrix = Array.initialise(4, 4, 0);

console.log(myMatrix);

{% endhighlight %}

Try running this code and you should get a 4 x 4 grid (matrix). Try assigning a value to one of the inner arrays <code>myMatrix[1][1] = 'a'</code>. This should only update one element in one array.

