---
title: "Introducing csval, an open source CSV data validator"
datePublished: Thu Oct 10 2019 22:01:01 GMT+0000 (Coordinated Universal Time)
cuid: ckroy0a4d0ph2fws1bq16gp98
slug: introducing-csval-an-open-source-csv-data-validator-98ccc40e376b
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627409468301/dodEdh1fH.png
tags: data, javascript, validation

---


csval is a command-line tool and a JavaScript library that can check CSV files against a set of validation rules. Some of the rules you can check for include…

* CSV file is actually valid itself and can be parsed

* Presence of required fields

* Mismatching types. For example, number vs string

* Minimum and maximum lengths

* Number ranges

* Values from a fixed set of options

* Regex pattern matching

* Much more. Check the [JSON Schema reference](https://json-schema.org/understanding-json-schema/reference/index.html) for more information

### Command line tool installation

csval can be installed and used as a CLI. It does, however, require [Node.js](https://nodejs.org/). If you have Node.js installed, you can install csval from the command line.

```
npm install -g csval
```


### Using the command line tool

Once installed, it can be called anywhere using the `csval` command. Give the path to a CSV file as the first argument.

```
csval mydata.csv
```


Since no rules were specified above, the file is only checked to make sure it can be parsed correctly. As long as it’s a valid CSV file, it will pass validation. The CLI will show errors if they exist. Otherwise, it will display a success message.

If you pass in a rules file, the CSV file will be validated against the rules.

```
csval mydata.csv myrules.json
```


Again, the CLI will show parsing errors if they exist. When a rules file is specified as it is above, the CLI will also display any validation errors. Otherwise, it will display a success message.

### The rules file

Rules files should follow the [JSON Schema](https://json-schema.org/understanding-json-schema/reference/index.html) format. It describes what you should expect in each row. Here’s an example.

```
{
  "type": "object",
  "properties": {
    "salary": {
      "type": "number"
    }
  }
}
```


Note that the line `"type": "object"` above is implied and can be left out if desired. I only included it here to demonstrate that this file should follow the JSON Schema format.

The rules above state that the “salary” field on each row must be a number.

This CSV file would pass:

```
name,salary
John,100000
Jane,150000
```


This CSV file would fail:

```
name,salary
John,100000
Jane,idk
```


### Another rules file example

Here’s another example of a rules file.

```
{
  "properties": {
    "age": {
      "type": "number",
      "minimum": 0
    }
  }
}
```


With those rules, this CSV file would pass:

```
name,age
John,30
Jane,50
```


But this one would fail.

```
name,age
John,30
Jane,-10
```


### Required fields

You can require certain fields, as well. Consider this rules file.

```
{
  "properties": {
    "age": {
      "type": "number"
    }
  },
  "required": ["age"]
}
```


This CSV file would pass:

```
name,age,salary
John,30,100000
Jane,50,150000
```


This one would fail:

```
name,salary
John,100000
Jane,150000
```


There are many other possible rules. See the [JSON Schema](https://json-schema.org/understanding-json-schema/reference/index.html) for more information.

### Programmatic API

csval also comes as a JavaScript library. You can install it with npm.

```
npm install csval
```


Use it in your project like so.

```
const csval = require("csval");

const main = async () => {
  const csvString= "name,age\nJohn,30";

  const rules = {
    properties: {
      name: {
        type: "string"
      }
    }
  };

  const parsed = await csval.parseCsv(csvString);
  const valid = await csval.validate(parsed, rules);

  // csval.validate will either throw an error or valid will be true
};

main();
```


The code above uses inline CSV data and rules. You can also read CSV data and rules from files.

```
const csval = require("csval");

const main = async () => {
  const csvString= await readCsv("path/to/file.csv");
  const rules = await readRules("path/to/rules.json");
  const parsed = await csval.parseCsv(csvString);
  const valid = await csval.validate(parsed, rules);

  // csval.validate will either throw an error or valid will be true
};

main();
```


### Open source development and contribution

csval is open source and I welcome all pull requests. The code is hosted on GitHub.


%[https://github.com/travishorn/csval]


If you’d like to fork or contribute, simply clone the git repository.

```
git clone https://github.com/travishorn/csval.git
```


Change into the directory.

```
cd csval
```


And install dependencies.

```
npm install
```


### Tests

csval has 100% test coverage at the moment. Tests are run via [Jest](https://jestjs.io/).

```
npm run test
```


You can view test coverage information, as well.

```
npm run test:coverage
```


### Linting

csval is written according to [Prettier code format](https://prettier.io/). You can lint all files for errors.

```
npm run lint
```


And automatically fix problems if possible.

```
npm run lint:fix
```


### License

csval is licensed under the MIT license. I welcome all pull requests and contributions at the repository on GitHub.

%[https://github.com/travishorn/csval]
