---
title: "fake-identity"
datePublished: Sun Sep 28 2014 16:52:00 GMT+0000 (Coordinated Universal Time)
cuid: ckrox1nyh0omcems18vwnc8yh
slug: fake-identity-2b9359c68187
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627409794716/5EtY4MAOa.png
tags: javascript, es6

---


This is the latest project I’ve open-sourced. It is a JavaScript module that you can use in the browser or Node to generate fake identity objects. The source for all the personal data comes from publicly available information that includes the most common items for each category. For example, the list of first names is from the Social Security Agency’s lists of top names.

*Generate random identity objects including name, address, etc. This may be useful if you are trying to fill your application with random personal data.*

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409793355/pSH0X88Wv.png)

* [Download .zip](https://github.com/travishorn/fake-identity/zipball/master)

* [Download .tar.gz](https://github.com/travishorn/fake-identity/tarball/master)

* [View on GitHub](https://github.com/travishorn/fake-identity)

## Features

* Generates a single random identity or multiple identities in an array.

* Includes first and last names, email address, phone number, street, city, state, zip code, date of birth, sex, company, and department.

* Email address is based off of name.

* First name matches sex.

* All names, streets, and cities are commonly found in the US.

* States are weighted on population, so more populous states appear more often.

* Zip codes are loosely based on state. Zip codes are weird so it’s not perfect, though.

* Date of birth will be between 18 and 60.

* Usable in the browser or Node.js.

* No dependencies.

## Installation

Add the following to your page:

```
<script src="dist/fake-identity.js"></script>
```


This will make a new global variable named Identity available.

## Browser

Add the following to your page:

```
<script src="dist/fake-identity.js"></script>
```


This will make a new global variable named Identity available.

## Node

Run

```
npm install fake-identity
```


Add the following to your application:

```
var Identity = require('fake-identity');
```


## Usage

Once installed, just use Identity.generate() in your scripts to get an identity object with random values. The returned object looks like this:

```
{
  firstName: "Amelia",
  lastName: "Wright",
  emailAddress: "awright@example.com",
  phoneNumber: "(555) 555-0155",
  street: "7327 Central Avenue",
  city: "Oxford",
  state: "TX",
  zipCode: "75045",
  dateOfBirth: Fri Jul 20 1962 00:00:00,
  sex: "female",
  company: "Contoso Pharmaceuticals",
  department: "Legal"
}
```


You can also pass a number to generate more than one identity. Multiple identity objects are returned in an array like so:

```
Identity.generate(3);

[
  {
    firstName: ...
    lastName: ...
    ...
  },
  {
    firstName: ...
    lastName: ...
    ...
  },
  {
    firstName: ...
    lastName: ...
    ...
  }
]
```


## Tests

Run

```
gulp test
```


Please note that, to run tests, you must clone the repository in git. Tests are not included in the NPM module.

*User icon by [OmmoZoubayr](http://www.ommozoubayr.esy.es/)*