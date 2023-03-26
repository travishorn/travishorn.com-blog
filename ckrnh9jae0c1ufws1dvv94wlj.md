---
title: "Using JavaScript to Work with Spreadsheets, Part 3: Accepting Arguments & Reading Files"
datePublished: Mon Jan 04 2021 23:03:06 GMT+0000 (Coordinated Universal Time)
cuid: ckrnh9jae0c1ufws1dvv94wlj
slug: using-javascript-to-work-with-spreadsheets-part-3-accepting-arguments-reading-files-63837e075f69
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627410119631/lErLM518q.png
tags: javascript, nodejs, excel

---


This is the third article in a series where we’ll be learning to use the powerful logic of a programming language like JavaScript to manipulate spreadsheet data.

In this article, we’ll talk about how to make a Node.js program, have it accept file paths as arguments, and how to read files from your hard drive.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410093248/mztl8Gfxg.png)

In order to make a reusable piece of code that can be used for many Excel files, we’ll want our project to take the form of a command-line interface (CLI). This means that users of our program will “interface” with it (use it) via the command line. You may be the sole person who will interface with this program, but it’s still a good idea to set it up to be as reusable as possible.

Create a new file in the project root folder called `index.js`. In VS Code, you can do this three different ways:

1. Click **File** > **New File**

1. Press the keyboard shortcut `Ctrl + N`

1. Click the **New File** icon in the Explorer

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410095184/GiVEDOmQ1.png)

Let’s create our first Node.js program by typing the following in `index.js`

```
console.log("Hello, World!");
```


Save the file.

Now run it by going back to the Terminal in VS Code ( `Ctrl + `` ) and typing `node index` and pressing Enter. This command tells Node.js to start and run the contents on `index.js`.

The contents of `index.js` tell node to simply “log” the words “Hello, World!” to the console (terminal).

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410096759/_NmZBsOG5.png)

We will eventually want our program to accept a parameter where we tell it which Excel file to open. For example, we want to be able to run `node index 'D:\Development\spreadsheets\Data Files\Company Database.xlsx'`. And we want our Node.js program to be able to pick up on the file path we gave it.

Parameters typed in the Terminal like this are called “arguments” in Node.js and can be accessed by the `process.argv` variable.

Instead of logging “Hello, World!,” let’s log the value of `process.argv`. Change `index.js` to the following:

```
console.log(process.argv);
```


Save the file. In the Terminal, run `node index` again.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410098502/GjrYebTMb.png)

Nice! Node.js logged the value of `process.argv` which is an array of strings. There are two values in this array. The first is the path to `node.exe` and the second is the path to `index.js`. Those are indeed the first and second arguments we typed as part of the command.

Let’s try adding an argument and see what Node.js logs. Run the following in the Terminal.

```
node index thirdargument
```


![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410099908/BoM_mOxr5.png)

The array now contains three strings, the third one being the third thing we typed as part of the command.

How about we pass it a path to our spreadsheet file. Your path may be different if your file is in another location.

```
node index 'D:\Development\spreadsheets\Data Files\Company Database.xlsx'
```


![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410101338/2kZq-HdnV.png)

What if we wanted to only log the file path without the other arguments? Meaning that we only want to log the third argument.

Well, in your JavaScript, you can tell Node.js to only print the value of `process.argv` at a specific index. Remember that JavaScript is zero-indexed, so the first argument can be accessed via `process.argv[0]`.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410102793/tN9oRbAAJ.png)

The second argument can be accessed via `process.argv[1]`.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410104243/RoBjWQk02.png)

And of course, the third argument is accessed at `process.argv[2]`.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410105907/O4GmftZlZ.png)

Now that we can get our file path by itself, let’s learn how to read files in Node.js.

Instead of reading a complex file like one saved in XLSX format, let’s start with a simple TXT file.

Create a file called `proverb.txt` anywhere. I’ll place mine in the `Data Files` folder I made.

Tip: You might as well create this file using VS Code. There’s no reason to switch to another editor like Notepad. Remember to start a new file, you can press `Ctrl + N`.

Inside `proverb.txt`, type whatever proverb you like.
> Be not afraid of growing slowly, be afraid only of standing still.

Save the file.

Back in `index.js` we’re going to “require” the `fs` module. This is a built-in module that comes with Node.js that allows you to access the computer’s file system (fs).

Delete everything out of `index.js` and type this:

```
// Require the fs module
const fs = require("fs");
```


Now the variable `fs` is loaded with all the functions is [Node.js’s fs module](https://nodejs.org/docs/latest-v14.x/api/fs.html).

Note: I’m saying “variable” here but we’re actually using a “constant” (const). If you’ve ever used `var = "something";` in JavaScript before, `const` is very similar, but it is “constant” (doesn’t change) instead of “variable” (can change). I know that I am not going to want to reassign `fs` to any other value, so I’m using `const` instead of `var`.

The `fs` module contains a useful function called `[readFile`](https://nodejs.org/docs/latest-v14.x/api/fs.html#fs_fs_readfile_path_options_callback) that accepts a file path to read. You use it like this:

```
fs.readFile("path to file");
```


How can we get a reference to our file path again? That’s right: `process.argv[2]`.

```
const inputPath = process.argv[2];
fs.readFile(inputPath);
```


The whole `index.js` now looks like this:

```
// Require the fs module
const fs = require("fs");

// Third argument should contain a file path. Get a reference to it
const inputPath = process.argv[2];

// Read the file at the path
fs.readFile(inputPath);
```


So what happens when we try to run the program and read our proverb?

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410107418/-yobsL4pw.png)

An error. This part of the error:

```
at Object.<anonymous> (D:\Development\spreadsheets\index.js:8:4)
```


Tells us that something happened in `index.js` on line `8` column `4`.

It looks like that’s the `readFile` function throwing the error. `readFile` is erroring out saying:

```
Callback must be a function. Received undefined
```


If we look at [the documentation for fs.readFile](https://nodejs.org/docs/latest-v14.x/api/fs.html#fs_fs_readfile_path_options_callback), we see that it is expecting (up to) three arguments.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410109002/uJvDchQaM.png)

The first is the path to the file. The second is optional (notice it’s in brackets) and the third is a “callback.”

Look back at our error. It says that `callback` *must* be a function. As our code is now, it’s undefined. Meaning we didn’t even provide a value at all. We only gave `readFile` a single argument.

Okay, without even going into what a callback is, we know it has to be a function. So let’s make a new (empty) function.

```
function readFileCallback() {
    // Something will go in here later
}
```


And let’s give `readFile` what it wants: three arguments — a path, options, and a callback function.

```
fs.readFile(inputPath, "", readFileCallback);
```


Notice the path is still the same, we’re just adding two additional arguments.

The options is simply an empty string since we don’t really care to set additional options.

The callback is a reference to our `readFileCallback` function.

`index.js` now looks like this:

```
// Require the fs module
const fs = require("fs");

// Third argument should contain a file path. Get a reference to it
const inputPath = process.argv[2];

function readFileCallback() {
    // Something will go in here later
}

// Read the file at the path
fs.readFile(process.argv[2], "", readFileCallback);
```


Notice I defined `readFileCallback()` above `fs.readFile()`. I like to define variables and functions *before* they’re called. It can prevent issues in complex code.

Save the file and run it again. The result?

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410110520/BfpvTz4Ni.png)

No more error, but not much else. The program just exits.

So what’s happening? `fs.readFile()` reads the file. But when it finishes, we’re not *doing* anything with the result. That’s what the callback is for.

In many Node.js built-in functions, when the function completes it’s job, it fires a “callback” function that you provide. In this case, we provided that callback function, but it’s just empty and nothing else happens.

In Node.js there is a strong convention that callback functions have two arguments: the first is always an error object (may be `null`), and the second is the result of the function call. In this case, that result would be the data in the file.

In fact, you can see [again in the documentation](https://nodejs.org/docs/latest-v14.x/api/fs.html#fs_fs_readfile_path_options_callback) that this is indeed the signature of that callback.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410112319/4G9QBerU4.png)

So our callback should really look like this:

```
// Used as a callback when a file is read
function readFileCallback(err, data) {
    // Log the data
    console.log(data);
}
```


We’re not worrying about the `err` argument just yet. We’re just going to log the `data`.

Save and run the file.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410114114/V6Ekj3DPt.png)

Okay getting closer, but we were expecting the text in the file, not `&lt;Buffer 42 65 20 6e 6f...&gt;`. It looks like the data returned is a Buffer.

We can check [the Node.js documentation to see what a Buffer is](https://nodejs.org/docs/latest-v14.x/api/buffer.html). It is a type of object used to represent sequences of bytes. I see in the documentation that buffers have a method called `toString()` which will decode the buffer into a string.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410115605/1tMobXBGp.png)

This method doesn’t have any required arguments. Let’s try using it.

```
// Require the fs module
const fs = require("fs");

// Third argument should contain a file path. Get a reference to it
const inputPath = process.argv[2];

// Used as a callback when a file is read
function readFileCallback(err, data) {
    // Convert the file's data buffer to a string
    const fileContents = data.toString();

    // Log the file contents
    console.log(fileContents);
}

// Read the file at the path
fs.readFile(process.argv[2], "", readFileCallback);
```


![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410117611/4biwewBQR.png)

Perfect. Our program can now read and display the contents of any text file we point it towards.

In the next article, we’ll learn how to add dependencies from the npm registry and we’ll add some robustness to our interface.