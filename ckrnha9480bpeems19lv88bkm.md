---
title: "Using JavaScript to Work with Spreadsheets, Part 4: Making the CLI More Robust"
datePublished: Mon Jan 11 2021 23:03:30 GMT+0000 (Coordinated Universal Time)
cuid: ckrnha9480bpeems19lv88bkm
slug: using-javascript-to-work-with-spreadsheets-part-4-making-the-cli-more-robust-92bbc0a45f27
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627410089805/xLowHt8yT.png
tags: javascript, nodejs, excel

---


This is the fourth article in a series where we’ll be learning to use the powerful logic of a programming language like JavaScript to manipulate spreadsheet data.

In this article, we’ll talk about how to add a dependency from the npm registry and we’ll add some robustness to our interface.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410071107/0pR0tEwfI.png)

Being able to accept arguments from the command line already places our program in the command-line interface (CLI) category. That is, our program is run by typing commands on the command line AKA terminal.

Right now, our CLI has no documentation and no ability accept options. It’s entirely possible to build your own system for looking at `process.argv` and parsing out options, adding commands for getting help with the program, etc. However, some very smart people have already written code which handles this and they’ve shared it with the world on Node.js’s npm registry.

There are many options on the registry for managing CLI’s. In this project, we’ll use the very popular [Commander](https://www.npmjs.com/package/commander). Installing it is easy.

In VS Code, with your project folder opened, run the following command in the Terminal ( `Ctrl + `` ).

```
npm i commander
```


This tells npm (Node Package Manager) to install the package by the name of “commander.” Now that it’s installed, we can use it in our project. Our project now “depends” on Commander and therefor Commander is considered a “dependency” of our project.

If you open up `package.json`, you can even see Commander listed in the `dependencies` section.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410073045/Ux8Hp9i_P.png)

Also you will notice that a new folder was created in your project called `node_modules` . This folder contains all the code for your dependencies.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410074632/JDdNSao2-.png)

And finally you will notice a new file called `package-lock.json`. This file helps npm keep track of the exact versions of dependencies that are installed.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410076043/rpp9SekQW.png)

Before we implement Commander into our file, let’s do a small bit of housekeeping. At the moment, `fs.readFile()` will be called every time we run the program. Instead, we want to move this process into a function that we can call later.

Create a new function called `logAction` which accepts a file path as an argument.

```
// Function for taking a file path and logging the contents
function logAction(path) {
    // Read file at the path
}
```


Move the line of code that reads files into this new function.

```
// Function for taking a file path and logging the contents
function logAction(path) {
    // Read the file at the path
    fs.readFile(process.argv[2], "", readFileCallback);
}
```


Change the line so that `fs.readFile` reads whatever path is passed into the function.

```
// Function for taking a file path and logging the contents
function logAction(path) {
    // Read the file at the path
    fs.readFile(path, "", readFileCallback);
}
```


Since our `readFileCallback` function isn’t used anywhere but inside this `logAction` function, we can actually nest it inside. Structuring the code this way keeps everything tidy.

```
// Function for taking a file path and logging the contents
function logAction(path) {
    // Used as a callback when a file is read
    function readFileCallback(err, data) {
        // Convert the file's data buffer to a string
        const fileContents = data.toString();

        // Log the file contents
        console.log(fileContents);
    }

    // Read the file at the path
    fs.readFile(path, "", readFileCallback);
}
```


I know that this new `logAction()` function isn’t being used yet. But it will be shortly.

The last bit of housekeeping is to remove the line where we define `inputPath`. It is no longer used at all.

```
// Delete this line:
const inputPath = process.argv[2];
```


Near the top of `index.js`, require Commander.

```
// Require outside packages
const fs = require("fs");
const commander = require("commander");
```


And at the very bottom of the file, tell Commander to parse our arguments.

```
// Parse the CLI arguments with Commander
commander.program.parse(process.argv);
```


NOTE: It’s very important to keep this line at the very bottom of the file. This is the code that tells Commander to actually parse the command at the very end after we’ve described what we want it to do.

If you save the file and run it now, nothing happens because we move the file reading component inside a function that never gets called.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410077498/qK6nSSE3Og.png)

However, you can start to see some of the features of Commander if you run `node index --help`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410079038/U_VXdbZGo.png)

This help section will actually automatically fill out as we tell Commander more about our program.

You can tell Commander about commands you want your program to have by using `commander.program.command()`.

Important: Make sure to place this and all further code **above** `commander.program.parse(process.argv);`.

```
// A Commander command for reading files and logging the contents
commander.program
  .command("log <path>")
  .description("Log a text file to the console")
  .action(logAction);
```


Notice how the action of this command is calling `logAction`. Commander will automatically pass the value of `&lt;path&gt;` to this function.

Try viewing the help again.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410080468/copYctFWr.png)

Commander automatically generated the help command and added information about our new `log` command.

We can use the `log` command like so:

```
node index log 'D:\Development\spreadsheets\Data Files\proverb.txt'
```


![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410081829/xoagUG7-N.png)

If we try to run it without a path, a friendly error appears.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410083509/_gYuL46Pn.png)

One thing we haven’t tied up yet is what happens when a path is given, but the path doesn’t actually exist. What if someone tries…

```
node index log 'C:\i-dont-exist.txt'
```


![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410085324/TaBPq1PpL.png)

That’s not a very friendly error. It’s telling us that the method `toString()` can’t be read from `undefined`. Since we are trying to do `data.toString()` that must mean `data` is undefined. `data` should be defined when `fs.readFile()` calls the `readFileCallback()` function. Here’s another look at that function again.

```
// Used as a callback when a file is read
function readFileCallback(err, data) {
    // Convert the file's data buffer to a string
    const fileContents = data.toString();

    // Log the file contents
    console.log(fileContents);
}
```


Aha. `readFileCallback()` takes two arguments: `err` and `data`. `data` isn’t defined, but I bet `err` is. Let’s write logic to handle this case.

```
// Used as a callback when a file is read
function readFileCallback(err, data) {
    if (err !== null) {
        console.error(err);
    } else {
        // Convert the file's data buffer to a string
        const fileContents = data.toString();

        // Log the file contents
        console.log(fileContents);
    }
}
```


This code says that if `err` isn’t `null`, log the `err` object to the console. I’m using `console.error` here instead of `console.log`. They mostly do the same thing, but `console.error` is more correct and it can behave differently depending on the environment.

Else, if `err` is `null`, run the code just as before.

Now we can see what error `fs.readFile` is giving us.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410086782/bnf1BgWA0.png)

Code `ENOENT`, which means the file we were trying to access does not exist.

We can actually make this error a little more friendly. If an error exists, execute some `switch` logic based on the code. If the code is `ENOENT`, log a friendly error message we wrote ourselves. Otherwise if it’s some other code we haven’t encountered, just show the ugly error message.

```
if (err !== null) {
    switch (err.code) {
        case "ENOENT":
            console.error("That file doesn't exist.");
            break;
        default:
            console.error(err.message);
    }
}
```


The reason I used `switch` here was so I can make other friendly error messages later if needed.

Running the program on a nonexistent file now produces a better error message.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410088233/Xa5fTL_FH.png)

Now that we know how to add commands to our program using Commander, we can keep extending our program to do different things depending on our needs.

In the next article, we’ll go over how to read and parse XLSX files.