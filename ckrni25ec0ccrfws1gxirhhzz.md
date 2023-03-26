---
title: "Composition + Immutability in JavaScript"
datePublished: Fri Nov 11 2016 22:01:01 GMT+0000 (Coordinated Universal Time)
cuid: ckrni25ec0ccrfws1gxirhhzz
slug: composition-immutability-in-javascript-d7c2fc6bbaa0
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627409922033/rg5ogSSlE.jpeg
tags: javascript, web-development, immutable

---


RJ Zaworski wrote an excellent introduction to Composition in JavaScript. I highly recommend it and it is a prerequisite to this article. I’m taking RJ’s article and extending it to use immutability.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409919985/C7h2zy1bR.jpeg)

On top of [composition](https://en.wikipedia.org/wiki/Function_composition_(computer_science)), immutability can further simplify application development on complex projects.

In this post, we’ll build a SortedList object to which you can add items. It will include a display() function that will always display the list sorted using whatever custom sorting function we describe.

JavaScript’s immutability support is limited. To extend it, we’ll use Facebook’s fantastic [Immutable.js](https://facebook.github.io/immutable-js/). Before any of the code below, make sure to include it.

```
<script src="[https://cdnjs.cloudflare.com/ajax/libs/immutable/3.8.1/immutable.js](https://cdnjs.cloudflare.com/ajax/libs/immutable/3.8.1/immutable.js)"></script
```


To start off, we’ll make a whole assortment of [pure functions](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-pure-function-d1c076bec976#.v01hvs4fr) that will be the building blocks of our SortedList.

The first one is a function that accepts a list and a string. It will then return the same list with addition of the string.

```
function add(items, newItem) {
  return items.push(newItem);
}
```


The next one is a function that accepts a list and returns that same list in alphabetical order.

```
function sort(items) {
  return items.sort();
}
```


Conversely, we’ll make another function that returns a list in reverse-alphabetical order.

```
function reverseSort(items) {
  return items.sort().reverse();
}
```


The final pure function accepts a list and returns an array of the lists items.

```
function displayList(items) {
  return items.toArray();
}
```


With all those building block functions out of the way, we’ll make the actual SortedList constructor function. It will accept an object containing pure functions for adding, sorting, and displaying items in the sorted list. It exposes functions for doing these three operations.

```
function SortedList(components) {
  this._items = Immutable.List.of();
  this.adder = components.adder;
  this.sorter = components.sorter;
  this.getter = components.getter;
  
  this.add = function(newItem) {
    this._items = this.adder(this._items, newItem);
    this.sort();
  };
  
  this.sort = function() {
    this._items = this.sorter(this._items);
  };
  
  this.display = function() {
    return this.getter(this._items);
  };
}
```


Now let’s actually use the SortedList constructor to make an instance that will display items in alphabetical order.

```
var list = new SortedList({
  adder: add,
  sorter: sort,
  getter: displayList,
});
```


And another instance that will display a reverse-alphabetical list.

```
var rlist = new SortedList({
  adder: add,
  sorter: reverseSort,
  getter: displayList,
});
```


To test these two functions out, let’s add five items to them each. Notice the random order.

```
list.add('Kilo')
list.add('Tango')
list.add('Mike')
list.add('Alpha')
list.add('Romeo');

rlist.add('Romeo')
rlist.add('Quebec')
rlist.add('India')
rlist.add('Delta')
rlist.add('Charlie');
```


That’s it, you can display each list’s items with the display() function.

```
list.display();
// ["Alpha", "Kilo", "Mike", "Romeo", "Tango"]

rList.display();
// ["Romeo", "Quebec", "India", "Delta", "Charlie"]
```


Notice the entire time, we did not have to write code that would mutate an array or any other function/object directly. Instead, we wrote functions that accept data as an argument and return a mutated version of it.
