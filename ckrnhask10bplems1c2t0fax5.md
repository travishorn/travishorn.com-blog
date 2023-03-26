---
title: "Using JavaScript to Work with Spreadsheets, Part 5: Parsing XLSX Files"
datePublished: Mon Jan 18 2021 23:01:12 GMT+0000 (Coordinated Universal Time)
cuid: ckrnhask10bplems1c2t0fax5
slug: using-javascript-to-work-with-spreadsheets-part-5-parsing-xlsx-files-c5ac049568fd
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627410067592/SB3KGFTJw.png
tags: javascript, nodejs, excel

---


This is the fifth article in a series where we’ll be learning to use the powerful logic of a programming language like JavaScript to manipulate spreadsheet data.

In this article, we’ll talk about how to read and parse XSLX files using JavaScript in Node.js.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410058098/tmk-KlBzX.png)

Now that you understand how Node.js can be used to read files on your hard drive and how you can build commands for a command-line interface (CLI), let’s learn how to read files saved by Microsoft Excel.

You could certainly use `fs.readFile()` to read an XLSX file and write code to parse out the data found within. However, this is another case where very smart people have already done this complex work created an npm package based around it.

The package we’ll use is called [ExcelJS](https://www.npmjs.com/package/exceljs). Install it now.

```
npm i exceljs
```


Require it at the top of `index.js`

```
const exceljs = require("exceljs");
```


Below our first command, add another.

```
// A Commander command for listing first column of first sheet
commander.program
  .command("listFirstCol <path>")
  .description("List first sheet column A")
  .action(listFirstColAction);
```


The `action` here is going to call `listFirstColAction()` which doesn’t exist yet. Go ahead and create it.

```
// Function for reading XLSX, logging first column of first sheet
function listFirstColAction(path) {
    // List logic here
}
```


Reading the documentation for ExcelJS, I see that [reading XLSX files](https://github.com/exceljs/exceljs#reading-xlsx) is done like so:

```
// Create an instance of workbook to load data into
const workbook = new exceljs.Workbook();

// Read the file
workbook.xlsx.readFile(path) {
    .then(function(book) {
        // Do something with the workbook
    });
}
```


Once you have a reference to the workbook in the `book` variable, you can get a specific sheet.

```
// Get reference to first worksheet
const sheet = book.getWorksheet(1);
```


And finally you can log the values of the first column to the console.

```
// Log the values of first column
console.log(sheet.getColumn(1).values);
```


You may notice that some parts of ExcelJS are one-indexed. This is in contrast to native JavaScript which is [zero-indexed](https://en.wikipedia.org/wiki/Zero-based_numbering). The first sheet is at index 1. The first column is at index 1.

The whole `listFirstColAction()` function looks like this:

```
// Function for reading XLSX, logging first column of first sheet
function listFirstColAction(path) {
    // Create an instance of workbook to load data into
    const workbook = new exceljs.Workbook();

// Read the file
    workbook.xlsx.readFile(path)
        .then(function(book) {
            // Get reference to first worksheet
            const sheet = book.getWorksheet(1);

            // Log the values of first column
            console.log(sheet.getColumn(1).values);
        });
}
```


If you were wondering why we have to use `.then()` after reading the file, that’s just the convention that ExcelJS uses. It is a popular pattern called “promises” and it’s an alternative to the callback pattern we used before. Not all JavaScript libraries use promises and not all use callbacks either. However, some even support both!

Anyway, running this command gives us the expected result.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410060207/XMmIuxjwE.png)

Here’s what the full `index.js` looks like. It’s getting to be a decent size.

```
// Require outside packages
const fs = require("fs");
const commander = require("commander");
const exceljs = require("exceljs");

// Function for taking a file path and logging the contents
function logAction(path) {
    // Used as a callback when a file is read
    function readFileCallback(err, data) {
        if (err !== null) {
            switch (err.code) {
                case "ENOENT":
                    console.error("That file doesn't exist.");
                    break;
                default:
                    console.error(err.message);
            }
        } else {
            // Convert the file's data buffer to a string
            const fileContents = data.toString();

            // Log the file contents
            console.log(fileContents);
        }
    }

    // Read the file at the path
    fs.readFile(path, "", readFileCallback);
}

// Function for reading XLSX, logging first column of first sheet
function listFirstColAction(path) {
    // Create an instance of workbook to load data into
    const workbook = new exceljs.Workbook();

    // Read the file
    workbook.xlsx.readFile(path)
        .then(function(book) {
            // Get reference to first worksheet
            const sheet = book.getWorksheet(1);

            // Log the values of first column
            console.log(sheet.getColumn(1).values);
        });
}

// A Commander command for reading files and logging the contents
commander.program
  .command("log <path>")
  .description("Log a text file to the console")
  .action(logAction);

// A Commander command for listing first column of first sheet
commander.program
  .command("listFirstCol <path>")
  .description("List first sheet column A")
  .action(listFirstColAction);

// Parse the CLI arguments with Commander
commander.program.parse(process.argv);
```


That’s over 60 lines of code including comments. While it’s common for source code files to go way above this number of lines, I still see some value in splitting up our code into logical sections at this time.

`logAction()` and `listFirstColAction()` can be moved into their own files.

Create a new folder called `actions`. Inside it, create two files called `log` and `listFirstcol`.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410061807/ANhi60Nnb.png)

Cut the entirety of the `logAction()` function and paste it into `actions/log.js`.

We need to tell Node.js to “export” that this function should be “exported” from the file any time it is “required” in another file. Simply add `module.exports = `before the function definition.

```
module.exports = function logAction(path) {
    // ... snip ...
};
```


Since this function uses `fs` we’ll need to make sure to require it at the top of this file. Add the require at the top of the file.

```
// Require outside packages
const fs = require("fs");
```


The whole `actions/log.js` file looks like this:

```
// Require outside packages
const fs = require("fs");

// Function for taking a file path and logging the contents
module.exports = function logAction(path) {
    // Used as a callback when a file is read
    function readFileCallback(err, data) {
        if (err !== null) {
            switch (err.code) {
                case "ENOENT":
                    console.error("That file doesn't exist.");
                    break;
                default:
                    console.error(err.message);
            }
        } else {
            // Convert the file's data buffer to a string
            const fileContents = data.toString();

            // Log the file contents
            console.log(fileContents);
        }
    }

// Read the file at the path
    fs.readFile(path, "", readFileCallback);
};
```


Repeat this same process for `actions/listFirstCol.js` keeping in mind that it doesn’t use the `fs` package, but does use `exceljs`.

```
// Require outside packages
const exceljs = require("exceljs");

// Function for reading XLSX, logging first column of first sheet
module.exports = function listFirstColAction(path) {
    // Create an instance of workbook to load data into
    const workbook = new exceljs.Workbook();

    // Read the file
    workbook.xlsx.readFile(path)
        .then(function(book) {
            // Get reference to first worksheet
            const sheet = book.getWorksheet(1);

            // Log the values of first column
            console.log(sheet.getColumn(1).values);
        });
};
```


Now that `index.js` doesn’t use `fs` nor `exceljs`, you can remove the `require` lines for them.

Since we moved each of our two actions into their own files, `index.js` is much smaller now.

```
// Require outside packages
const commander = require("commander");

// A Commander command for reading files and logging the contents
commander.program
  .command("log <path>")
  .description("Log a text file to the console")
  .action(logAction);

// A Commander command for listing first column of first sheet
commander.program
  .command("listFirstCol <path>")
  .description("List first sheet column A")
  .action(listFirstColAction);

// Parse the CLI arguments with Commander
commander.program.parse(process.argv);
```


But if you try to run the program, you’ll see an error.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410063732/bUQMiR2CH.png)

`logAction` is being used in our program, but it’s not defined anywhere. That’s right, because we moved it.

We can simply `require` it from the file we saved. Do this for both actions.

```
// Require outside packages
const commander = require("commander");
const logAction = require("./actions/log");
const listFirstColAction = require("./actions/listFirstCol");
```


Save the file, try it again, and it’s working!

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410065890/wcXr7D76Q.png)

In this article, we’ve learned how to read and parse XLSX files using ExcelJS. So far we’ve only used a very small set of features from this package. You may consider [reading the documentation and seeing everything ExcelJS can do](https://github.com/exceljs/exceljs#readme).

In the next article, we’ll try generating some summary data from our spreadsheet.