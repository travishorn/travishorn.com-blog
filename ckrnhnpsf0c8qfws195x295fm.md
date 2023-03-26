---
title: "JavaScript Reference: Objects"
datePublished: Wed Aug 03 2016 22:01:02 GMT+0000 (Coordinated Universal Time)
cuid: ckrnhnpsf0c8qfws195x295fm
slug: javascript-reference-objects-4b15698410fc
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627409971786/aC0Mm7ljv.jpeg
tags: oop, javascript, programing

---


This post is part of a project to move my old reference material to my blog. Before 2012, when I accessed the same pieces of code or general information multiple times, I would write a quick HTML page for my own reference and put it on a personal site. Later, I published these pages online. Some of the pages still get used and now I want to make them available on my blog.

![Photo by [Ravali Yan](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409970072/8U1YVyph6.html)](https://cdn-images-1.medium.com/max/12000/1*ZQpFkUlcr1zNqFUUMmez1Q.jpeg)*Photo by [Ravali Yan](https://unsplash.com/@ravali)*

### Bracket Notation

```
// Create object using bracket notation
var testData = new Object();
testData["districtName"] = "Central School District";
testData["schoolName"] = "Lincoln Elementary";
testData["schoolYear"] = 2011;

// Access property using string stored in variable
var propertyNameStringVariable = "districtName";
testData[propertyNameStringVariable] = "Central School District";

// Loop through properties using bracket notation
function showProps(obj) {
  var result = "";
  for (var i in obj) {
    result += i + " = " + obj[i] + "<br>";
  }
  return result;
}

// Write the result of showProps to screen
document.write(showProps(testData));
```


### Constructor Function

```
// Create constructor function and create objects using it
// Strong convention to use capital initial letter
function TestData(districtName, schoolName, schoolYear) {
  this.districtName = districtName;
  this.schoolName = schoolName;
  this.schoolYear = schoolYear;
}

var data1 = new TestData(
  "Central School District",
  "Lincoln Elementary",
  2011
);

var data2 = new TestData(
  "Central School District",
  "Jefferson Elementary",
  2011
);
    
// Add property to previously defined object type
TestData.prototype.semester = null;
data1.semester = 1;

// Loop through properties using bracket notation
function showProps(obj) {
  var result = "";
  for (var i in obj) {
    result += i + " = " + obj[i] + "<br>";
  }
  return result;
}

// Write the result of showProps to screen
document.write(showProps(data1));
```


### Dot Notation

```
// Create object using dot notation
var testData = new Object();
testData.districtName = "Central School District";
testData.schoolName = "Lincoln Elementary";
testData.schoolYear = 2011;

// Loop through properties using bracket notation
function showProps(obj) {
  var result = "";
  for (var i in obj) {
    result += i + " = " + obj[i] + "<br>";
  }
  return result;
}

// Write the result of showProps to screen
document.write(showProps(testData));
```


### Getters & Setters

```
// Define one getter and one setter
var testData = {
  "districtName": "Central School District",
  "schoolName": "Lincoln Elementary",
  "schoolYear": 2011,
  get names() {
    return this.districtName + ", " + this.schoolName;
  },
  set incYear(x) {
    this.schoolYear += x;
  }
};

// Call incYear setter
testData.incYear = 1;

// Write getter to screen
document.write(testData.names + "<br>");

// Write schoolYear property to screen after mutating with setter
document.write(testData["schoolYear"]);
```


### Nesting Constructor Function

```
// Create object within object using construction functions
function School(name, level) {
  this.name = name;
  this.level = level;
}

function TestData(districtName, school, schoolYear) {
  this.districtName = districtName;
  this.school = school;
  this.schoolYear = schoolYear;
}

var lincoln = new School(
  "Lincoln",
  "Elementary"
);

var data1 = new TestData(
  "Central School District",
  lincoln,
  2011
);

// Loop through properties using bracket notation
function showProps(obj) {
  var result = "";
  for (var i in obj) {
    result += i + " = " + obj[i] + "<br>";
  }
  return result;
}

// Write the result of showProps to screen
document.write("data1:<br>" + showProps(data1) + "<br>");
document.write("lincoln:<br>" + showProps(lincoln) + "<br>");
```


### Nesting Object Initializer

```
// Object within object using initializer
var testData = {
  "districtName": "Central School District",
  "school": {
    "name": "Lincoln",
    "level": "Elementary"
  },
  "schoolYear": 2011
};

// Loop through properties using bracket notation
function showProps(obj) {
  var result = "";
  for (var i in obj) {
    result += i + " = " + obj[i] + "<br>";
  }
  return result;
}

// Write the result of showProps to screen
document.write(showProps(testData));
```


### Methods

```
function TestData(districtName, schoolName, schoolYear) {
  this.districtName = districtName;
  this.schoolName = schoolName;
  this.schoolYear = schoolYear;
  this.displayData = function(){
    document.write("displayData:<br>" + this.schoolName + "'s " + this.schoolYear + " school year at " + this.districtName + ".");
  };
}

var data1 = new TestData(
  "Central School District",
  "Lincoln Elementary",
  2011
);

// Loop through properties using bracket notation
function showProps(obj) {
  var result = "";
  for (var i in obj) {
    result += i + " = " + obj[i] + "<br>";
  }
  return result;
}

// Write the result of showProps to screen
document.write("data1:<br>" + showProps(data1) + "<br>");

// Call displayData method
data1.displayData();
```


### Object Initializer

```
// Create object using object initializer (literal notation)
var testData = {
  "districtName": "Central School District",
  "schoolName": "Lincoln Elementary",
  "schoolYear": 2011
};

// Loop through properties using bracket notation
function showProps(obj) {
  var result = "";
  for (var i in obj) {
    result += i + " = " + obj[i] + "<br>";
  }
  return result;
}

// Write the result of showProps to screen
document.write(showProps(testData));
```
