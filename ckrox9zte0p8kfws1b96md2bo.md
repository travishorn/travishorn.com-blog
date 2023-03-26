---
title: "Using Computed Getters & Setters in Vue"
datePublished: Tue May 02 2017 22:01:02 GMT+0000 (Coordinated Universal Time)
cuid: ckrox9zte0p8kfws1b96md2bo
slug: using-computed-getters-setters-in-vue-57f978b10bd7
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627409742931/aoy3bOQRr.jpeg
tags: javascript, web-development, vuejs, frontend-development

---


Vue.js supports getters and setters on computed variables. You can use these variables to provide 2-way data binding. Let’s use this feature to write an app that converts a set of checkboxes (true/false selected values) to binary and vice-versa.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409741060/p2RaeLGD8y.jpeg)

### What we’ll be building

As you check the boxes below, watch the text input at the bottom change. You can also change the text input directly to check/uncheck the boxes. This type of conversion could be used to allow users to “import” and “export” options via their clipboard. You could also use this method to store the binary version in localStorage (and retrieve it later to restore checked options).

%[https://codepen.io/travishorn/pen/YVQEKJ]

I’ll be using [Bootstrap 4](https://v4-alpha.getbootstrap.com/) for styling so we won’t have to write any CSS. The HTML setup looks like this:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

    <title>Convert Set of True/False Values to Binary</title>

    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0-alpha.6/css/bootstrap.min.css">
  </head>
  <body>
    <div id="app"></div>

    <script src="https://unpkg.com/vue@2.3.2"></script>
    <script src="main.js"></script>
  </body>
</html>
```


Inside the `&lt;div id="app"&gt;` let’s build our app template.

```
<div class="form-check" v-for="pet in pets">
  <label class="form-check-label">
    <input class="form-check-input" type="checkbox" v-model="pet.selected">
    {{ pet.name }}
  </label>
</div><!-- /form-check -->

<input class="form-control" v-model="binary">
```


That’s it for the template! The first block loops through a `pets` array and creates a checkbox for each one. By setting the `v-model` to `pet.selected`, Vue will set the checkbox to be selected if the `pet.selected` value is `true`.

The second block (line) is a text input bound to the `binary` variable. This binary variable will actually be a [computed property](https://vuejs.org/v2/guide/computed.html). Normally, computed properties in Vue are read-only. But we can use a setter (and getter) to support 2-way binding.

We’ll build our app in `main.js`:

```
new Vue({
  el: '#app',
  data: {
    pets: [], // coming soon
  },
  computed: {
    binary: {
      get: function () {}, // coming soon
      set: function () {}, // coming soon
    },
  },
});
```


Our `pets` array is empty. Fill it up with some common pets. Our template is expecting a `name` and `selected` property for each one.

```
pets: [
  { name: 'Bird',    selected: false },
  { name: 'Cat',     selected: false },
  { name: 'Dog',     selected: false },
  { name: 'Fish',    selected: false },
  { name: 'Horse',   selected: false },
  { name: 'Rabbit',  selected: false },
  { name: 'Reptile', selected: false },
  { name: 'Rodent',  selected: false },
  { name: 'Turtle',  selected: false },
],
```


None of them are selected by default.

To `get` a binary representation of this list (selected/not selected), we simply loop through the array and add a 1 or 0 for if `selected` is `true` or `false` respectively.

```
binary: {
  get: function () {
    var binString = '';

    this.pets.map(function(pet) {
      binString += (pet.selected ? 1 : 0);
    });

    return binString;
  },
  ...
},
```


To take a binary string and set our checkboxes, we must loop through the string and `set` the `selected` properties accordingly. If `1`, then `true`. Else, then `false`.

```
binary: {
  ...
  set: function (binString) {
    var i;

    for (i = 0; i < binString.length; i++) {
      this.pets[i].selected = binString[i] === '1' ? true: false;
    }
  },
},
```


The app is done! Check the boxes and watch the binary representation change. Change the binary and watch the boxes check/uncheck themselves.

%[https://codepen.io/travishorn/pen/YVQEKJ]

### Here’s an interesting idea

Binary numbers can be converted to decimal. For example, `101101110` (binary, 9 characters in a JS string) can also be expressed as `366` (decimal, 3 characters in a JS string). As a form of compression, you could convert the binary to decimal before displaying it to the user. Then, convert it back when they paste it in the text input. In fact, you could convert it to higher bases as well for even more compression. What about base 64?