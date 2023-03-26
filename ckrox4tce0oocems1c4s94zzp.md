---
title: "Responsive Buttons with Bootstrap"
datePublished: Thu Oct 12 2017 22:01:00 GMT+0000 (Coordinated Universal Time)
cuid: ckrox4tce0oocems1c4s94zzp
slug: responsive-buttons-with-bootstrap-7c2a7c144308
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627409784898/_CXz08V5a.jpeg
tags: bootstrap, web-design, web-development, responsive-web-design

---


Using Bootstrap, I found myself wanting to have a button that will change widths depending on the screen size. Here is what I believe to be the best solution. It works with Bootstrap 4 right out of the box.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409782411/o7VAWLJ4y.jpeg)

The goal is to get a button to have a limited width unless the screen is small, in which case the button should expand to fit the entire width of the container.

I discovered this can be accomplished by adding the `[btn-block` class](https://getbootstrap.com/docs/4.0/components/buttons/#sizes) to the button and placing it inside of a `&lt;div&gt;` with the appropriate `col` classes to size it at different breakpoints.
> btn-block creates block level buttons — those that span the full width of a parent.

Here’s a simple example:

```
<div class="container my-5">

  <h1>Mailing List</h1>

  <div class="form-group">
    <input class="form-control" placeholder="Your email address">
  </div>

  <div class="row">
    <div class="col-md-4 col-lg-2">
      <button class="btn btn-primary btn-block">Sign up</button>
    </div>
  </div>
  
</div>
```


Notice…

1. The button is inside a `<div>` with `col-md-4` and `col-lg-2` classes. This means the `<div>` will span 12/12 (100%) of its container by default, 4/12 (1/3) of its container on medium screens, and 2/12 (1/6) of its container on large screens.

1. The `<button>` has the `btn-block` class, which spans the full width of its container (which is the previously mentioned `<div>`).

The end result can be seen in this pen:

%[https://codepen.io/travishorn/pen/oLgyPO]
