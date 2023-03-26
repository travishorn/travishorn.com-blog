---
title: "Populate <select> via AJAX"
datePublished: Fri Nov 04 2016 22:01:02 GMT+0000 (Coordinated Universal Time)
cuid: ckroxn17l0ouqems12py0ersu
slug: populate-select-via-ajax-f2f088a47750
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627409614519/nWTB20SRf.jpeg
tags: forms, javascript, web-development, ajax, frontend-development

---


I recently ran into a situation where I had a <select> input and I wanted to populate it with a list of options from an API endpoint. In fact, I had multiple <select>s on the same page and I wanted to do it for all of them. I’ll show you the process I went through to build an easy solution.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409611841/gaft8iQcj.jpeg)

Ultimately, what we’ll build is a solution that enables us to add `data-*` attributes to our `&lt;select&gt;` that describe how to fetch our options. Like so:

```
<select
  id="userId"
  class="form-control"
  name="userId"
  data-source="[https://jsonplaceholder.typicode.com/users](https://jsonplaceholder.typicode.com/users)"
  data-valueKey="id"
  data-displayKey="name">
</select>
```


Notice the…

* `data-source` that contains the URL to the API endpoint

* `data-valueKey` that tells us what key to use (from the returned JSON) for the `&lt;option&gt;`’s value

* `data-displayKey` that tells us what key to use for the displayed text

For simplicity’s sake, I’ll be using [jQuery](https://jquery.com/). However, you could also re-write this using [axios](https://github.com/axios/axios) or vanilla JavaScript with [XMLHttpRequest](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) or the [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API).

Let’s start off by getting a reference to all of our `&lt;select&gt;`’s that have a `data-source` attribute.

```
$('select[data-source]).each(function() {
  // The rest of the code will go here
});
```


Inside this function, we’ll grab a copy of `this` inside a jQuery wrapper. In this instance, `this` is a reference to the `&lt;select&gt;` element.

```
var $select = $(this);
```


Now, — and this part is optional — I like to add an empty `&lt;option&gt;` that the `&lt;select&gt;` will default to.

```
$select.append('<option></option>');
```


Next, we’ll pull the list of items via AJAX.

```
$.ajax({
  url: $select.attr('data-source');
}).then(function(options) {
  // Handle the API response here
});
```


Notice we’re getting the URL from the `&lt;select&gt;`’s data-source attribute. Also notice, since `$.ajax()` is a [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise), we’re attaching a `.then()` function to the end that we can use to determine what happens once the API response is returned.

Inside the `.then()` function, let’s loop through our options.

```
options.map(function(option) {
  // Handle each option here
});
```


For each option, we’ll create a new jQuery-wrapped `&lt;option&gt;` element, set its value, and set the display text.

```
var $option = $('<option>');

$option
  .val(option[$select.attr('data-valueKey')])
  .text(option[$select.attr('data-displayKey']);
```


Finally, we’ll append the `&lt;option&gt;` to the select `&lt;select&gt;`.

```
$select.append($option);
```


The finished product (19 lines of code) looks like this:

```
$('select[data-source]').each(function() {
  var $select = $(this);
  
  $select.append('<option></option>');
  
  $.ajax({
    url: $select.attr('data-source'),
  }).then(function(options) {
    options.map(function(option) {
      var $option = $('<option>');
      
      $option
        .val(option[$select.attr('data-valueKey')])
        .text(option[$select.attr('data-displayKey')]);
      
      $select.append($option);
    });
  });
});
```

%[https://codepen.io/travishorn/pen/wzxxkN]

All `&lt;select&gt;` elements on the page will automatically have their `&lt;options&gt;` populated from the API in their `data-source`.