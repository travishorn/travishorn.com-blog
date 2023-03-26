---
title: "JavaScript Reference: Closures"
datePublished: Wed Aug 03 2016 22:01:01 GMT+0000 (Coordinated Universal Time)
cuid: ckrni3mxp0byxems1gm9p05nj
slug: javascript-reference-closures-a9b81695fdaf
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627409911515/2GMLUcafn.jpeg
tags: closure, programming, javascript

---


This post is part of a project to move my old reference material to my blog. Before 2012, when I accessed the same pieces of code or general information multiple times, I would write a quick HTML page for my own reference and put it on a personal site. Later, I published these pages online. Some of the pages still get used and now I want to make them available on my blog.

![Photo by [Todd Diemer](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409909643/BnvDimQmS.html)](https://cdn-images-1.medium.com/max/9792/1*eE0hSsS-BpzTQ4nlHFwMTQ.jpeg)*Photo by [Todd Diemer](https://unsplash.com/@todd_diemer)*

### Emulating Private Methods

```
<script>
    // Create single environment with 3 functions
    var makeCounter = function() {
      // Private variable and private function
      var privateCounter = 0;
      function changeBy(val) {
        privateCounter += val;
      }
      return {
        increment: function() {
          changeBy(1);
        },
        decrement: function() {
          changeBy(-1);
        },
        value: function() {
          return privateCounter;
        }
      }   
    };
    
    
    // Create closure from function
    counter = makeCounter();
    
    // Use functions of closure
    document.write(counter.value() + "<br>");
    
    counter.increment();
    counter.increment();
    
    document.write(counter.value() + "<br>");
    
    counter.decrement();
    
    document.write(counter.value());
  </script>
```


### Multiple Closures

```
<script>
    // Create the function
    function makeLevelAdder(level){
      return function(schoolName) {
        return schoolName + " " + level;
      };
    }
    
    // Create the closures
    var addElementary = makeLevelAdder("Elementary");
    var addMiddle = makeLevelAdder("Middle");
    
    // Print to screen using closures
    document.write(addElementary("Lincoln") + "<br>");
    document.write(addMiddle("Washington"));
  </script>
```


### Simple Example

```
<script>
    // Create a function
    function func() {
      var districtName = "Central School District";
      function displayName() {
        document.write(districtName);
      }
      displayName();
    }
    
    // Put the function in a closure and call it.
    var closure = func();
    closure();
  </script>
```
