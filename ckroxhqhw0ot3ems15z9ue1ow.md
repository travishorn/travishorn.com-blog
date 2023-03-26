---
title: "Building json2table: Turn JSON into an HTML table"
datePublished: Thu Sep 21 2017 22:01:01 GMT+0000 (Coordinated Universal Time)
cuid: ckroxhqhw0ot3ems15z9ue1ow
slug: building-json2table-turn-json-into-an-html-table-a57cf642b84a
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627409689989/43RWE4DSI.jpeg
tags: javascript, json, html5

---


Today we’ll build a dependency-free function that accepts…

* Data in a JSON array

* An optional space-separated list of classes

… then transforms the data and returns a string of HTML representing a table element with the specified classes.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409687686/cI0nPyto4.jpeg)

The data passed to this function should be an array of JavaScript objects, each with the same properties. For example:

```
[
  { country: 'China',         population: 1379510000 },
  { country: 'India',         population: 1330780000 },
  { country: 'United States', population:  324788000 },
  { country: 'Indonesia',     population:  260581000 },
  { country: 'Brazil',        population:  206855000 },
];
```


Notice each object has a `country` property and a `population` property.

Our function will be called `json2table` and accepts a `json` argument and an optional `classes` argument.

```
function json2table(json, classes) {
  // Everything goes in here
}
```


First we’ll set up some variables.

```
var cols = Object.keys(json[0]);
var headerRow = '';
var bodyRows = '';
```


We want one table column per property. Since it can be assumed (or error-checked later) that each object has the same properties, the first line above grabs the the property names (keys) from the first object in the array and stores it in a variable called `cols`. The other two variables are empty strings for now and we’ll build them out in a minute.

Next, we’ll make sure that if no `classes` argument is passed, that the `classes` variable contains an empty string. This will be important later when we build the HTML string.

```
classes = classes || '';
```


Now I’m going to define a utility function that capitalizes the first letter of a string.

```
function capitalizeFirstLetter(string) {
  return string.charAt(0).toUpperCase() + string.slice(1);
}
```


This function applies`.toUpperCase()` to `.charAt(0)` (the first character), then concatenates that with `.slice(1)` (the rest of the string).

Next, we loop over the list of keys and create column headers.

```
cols.map(function(col) {
  headerRow += '<th>' + capitalizeFirstLetter(col) + '</th>';
});
```


Now, the`headerRow` variable contains a string of `&lt;th&gt;` elements.

Next, loop over the array and build a string of HTML table rows.

```
json.map(function(row) {
  bodyRows += '<tr>;

  // To do: Loop over object properties and create cells

  bodyRows += '</tr>';
}
```


You can see how each row will begin with `&lt;tr&gt;` and end with `&lt;/tr&gt;`. Between each of those tags, we need to loop over the object properties and create cells (`&lt;td&gt;`s). This next part goes inside our loop above.

```
cols.map(function(colName) {
  bodyRows += '<td>' + row[colName] + '<td>';
});
```


Finally, we just need to return the whole HTML string, including any `classes` passed to the function.

```
return '<table class=" +
       classes +
       '"><thead><tr>' +
       headerRow +
       '</tr></thead><tbody>' +
       bodyRows +
       '</tbody></table>';
```


That’s it! If we pass our example arrow of objects from earlier…

```
var data= [
  { country: 'China',         population: 1379510000 },
  { country: 'India',         population: 1330780000 },
  { country: 'United States', population:  324788000 },
  { country: 'Indonesia',     population:  260581000 },
  { country: 'Brazil',        population:  206855000 },
];

json2table(data);
```


We get a string of HTML representing a table.

```
<!-- Formatted for clarity -->

<table class="">
  <thead>
    <tr>
      <th>Country</th>
      <th>Population</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>China</td>
      <td>1379510000</td>
    </tr>
    <tr>
      <td>India</td>
      <td>1330780000</td>
    </tr>
    <tr>
      <td>United States</td>
      <td>324788000</td>
    </tr>
    <tr>
      <td>Indonesia</td>
      <td>260581000</td>
    </tr>
    <tr>
      <td>Brazil</td>
      <td>206855000</td>
    </tr>
  </tbody>
</table>
```


On a webpage, you could place this anywhere by writing something like this:

```
<!-- HTML -->
<div id="tableGoesHere"></div>

// JavaScript
document.getElementById('tableGoesHere').innerHTML = json2table(data);
```

%[https://codepen.io/travishorn/pen/qXvaKa]
