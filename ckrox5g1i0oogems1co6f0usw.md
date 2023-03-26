---
title: "Creating a front-end for working with your API proxy"
datePublished: Tue Apr 05 2016 20:40:35 GMT+0000 (Coordinated Universal Time)
cuid: ckrox5g1i0oogems1co6f0usw
slug: creating-a-front-end-for-working-with-your-api-proxy-7843670c6022
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627409779213/tTHmae6Pr.jpeg
tags: javascript, nodejs, apis

---


This is part two of a two-part series on working with a third-party API with private keys. If you haven’t already, you may want to read part one.

Now that the proxy is running, we can create a front-end form for posting data to it. This form can be on any server, as we have used CORS to make the service available to any entry point.

![Photo by [aka Tman](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409777147/zMhkQnSY1_.html).](https://cdn-images-1.medium.com/max/3840/1*hYRT-0Jkig0_Btuc2T9lrg.jpeg)*Photo by [aka Tman](https://www.flickr.com/photos/rundwolf/).*

### The HTML

We’ll start with a simple boilerplate in HTML.

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta http-equiv="x-ua-compatible" content="ie=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">

    <title>Mailing List Signup</title>
  </head>
  <body>
    <div class="container">

    </div>
  </body>
</html>  
```


To keep things simple, add the stylesheet from Bootstrap to the &lt;head&gt;.

```
<link rel="stylesheet" href="[https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap.min.css](https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap.min.css)">
```


In the container, add a header.

```
<div class="page-header">
  <h1>Mailing List Signup</h1>
</div>
```


Below that, we’ll add a single “row” and single “column” that will use breakpoints for styling the form. It will show up full-width on small screens, and centered on larger ones.

```
<div class="row">
  <div class="col-md-6 col-md-offset-3">

  </div>
</div>
```


Inside this “column,” we’ll add a couple alert boxes that are hidden by default. These boxes will display error and success messages.

```
<div id="errorDiv" class="alert alert-danger" style="display:none;">
  <ul id="errorList" class="list-unstyled"></ul>
</div>

<div id="successDiv" class="alert alert-success" style="display:none;">
  <p>Thank you! You are signed up.</p>
</div>
```


Below those alert boxes, let’s add the form. It’s a simple form with one input (for email address) and a submit button. You may need to change the action attribute depending on what address and port your proxy server is running on.

```
<form id="signupForm" action="http://127.0.0.1:3000/contacts" method="post">
  <div class="form-group">
    <label for="emailAddress"Email address</label>
    <input id="emailAddress class="form-control" name="emailAddress" type="text" maxlength="100" required autofocus>
  </div>

  <button class="btn btn-primary btn-lg">Sign up</button>
</form>
```


With the form set up, we’ll need to add some JavaScript to handle submitting the for through AJAX. We’ll use the help of jQuery here. Include these two script tags just before the closing &lt;/body&gt; tag.

```
<script src="[https://code.jquery.com/jquery-1.12.2.min.js](https://code.jquery.com/jquery-1.12.2.min.js)"></script>
<script src="mailing-list-signup.js"></script>
```


### The JavaScript

Create a new file called mailing-list-signup.js. We’ll start with a jQuery document ready function and put the parser in strict mode.

```
$(function() {
  'use strict';

  // The rest of the code will go here.

});
```


I always like to store the DOM elements that I’ll be using in variables. Add the following variables:

```
var signupForm = $('#signupForm');
var errorDiv = $('#errorDiv');
var errorList = $('#errorList');
var successDiv = $('#successDiv');
```


We’re going to need a few functions for showing and hiding the alert boxes.

```
function showErrors(errors) {
  errors.map(function(error) {
    errorList.append('<li>' + error + '</li>');
  });

  errorDiv.show();
}

function hideErrors() {
  errorDiv.hide();
  errorList.html('');
}

function showSuccess() {
  successDiv.show();
}

function hideSuccess() {
  successDiv.hide();
}
```


We’ll also write a function to clear our form. Since the form only contains one input (and it’s a text input), we can write a short line to just set the value to blank.

```
function clearForm(form) {
  form.find('input[type=text]').val('');
}
```


Now, listen for form submission.

```
signupForm.on('submit', function(e) {
  // The rest of the code will go here.
});
```


Inside the form submission event, we’ll set up some variables for later use.

```
var emailAddress = $('#emailAddress').val();
var errors = [];
```


Next, we’ll prevent default form submission and hide any shown alert boxes.

```
e.preventDefault();
hideErrors();
hideSuccess();
```


We still need some error checking. There are many possibilities here, but for the sake of simplicity, I’ll just check for a blank email address.

```
if (!emailAddress) { errors.push('Please enter your email address.'); }
```


You can add more error checks and keep pushing errors into the errors variable, but I’ll just leave it for now. The next step is to see whether there were errors. If so, we need to display them.

```
if (errors.length) {
  showErrors(errors);
} else {
  // continue form submission
}
```


As long as there aren’t any errors we can submit the form through AJAX.

```
$.ajax({
  type: signupForm.attr('method'),
  url: signupForm.attr('action'),
  data: signupForm.serialize(),
})
.done(function(res) {
  // Do something with response
})
.fail(function(jqXHR, textStatus, errThrown) {
  // Do something with thrown error
});
```


To handle the AJAX request failing, we should add this code in the .fail function:

```
errors.push(errThrown);
showErrors(errors);
```


If the AJAX request goes through, the server may still throw an error, so we’ll handle that as well. This code should go in the .done function:

```
if (res.code !== 201) {
  errors.push(res.body);
  showErrors(errors);
} else {
  // If we get this far, no errors have been returned and we
  // can show success.
}
```


So if no errors were returned, let’s let the user know by showing the success alert and clearing the form for another possible use.

```
showSuccess();
clearForm(signupForm);
```


That should do it!

The code for both this front-end form and the back-end proxy can be found in [this GitHub repository](https://github.com/travishorn/fieldbook-proxy-example).