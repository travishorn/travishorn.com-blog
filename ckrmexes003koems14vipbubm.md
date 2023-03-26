---
title: "Zero to TypeScript Developer using Visual Studio Code on Ubuntu"
datePublished: Wed Jun 08 2016 22:01:03 GMT+0000 (Coordinated Universal Time)
cuid: ckrmexes003koems14vipbubm
slug: zero-to-typescript-developer-using-visual-studio-code-on-ubuntu-18ee721136f9
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627410486007/ugt4eBF-7.jpeg
tags: ubuntu, javascript, typescript

---


Let’s start from scratch, or rather — with a running installation of Ubuntu. From there, I’ll walk you through installing and setting up an environment to code with TypeScript. In a future post, I’ll walk though some of the basics of TypeScript that build upon JavaScript.

![Photo by [Luca Bravo](https://cdn.hashnode.com/res/hashnode/image/upload/v1627410483648/eFoEdR6mN.html)](https://cdn-images-1.medium.com/max/10144/1*AMQ7-bp78CV-aOvRaZ_8hw.jpeg)*Photo by [Luca Bravo](https://unsplash.com/@lucabravo)*

The bare minimum for this tutorial is a computer running Ubuntu. If you don’t yet have that, I’ll point you to [the official installation documentation](http://www.ubuntu.com/download/desktop/install-ubuntu-desktop).

The first thing you’ll need is Node.js. Obviously, if you already have it, you can skip this section. Otherwise…

1. Press Ctrl + Alt + T to bring up a **Terminal** window

1. Type **curl -sL [https://deb.nodesource.com/setup_4.x](https://deb.nodesource.com/setup_4.x) | sudo -E bash -** and press **Enter**

1. Type **sudo apt-get install -y nodejs** and press **Enter**

Node will be installed. When it completes, you can check by typing **node -v**. Now let’s install the TypeScript compiler.

1. In the same **Terminal** window, type **sudo npm install -g typescript** and press **Enter**

This single line is all that’s required to install the TypeScript compiler. When it completes, you can also check this by typing **tsc -v**. You may now close the **Terminal** window.

Node and the TypeScript compiler are installed now. Let’s install Visual Studio Code.

1. On the left side of your screen, click on the icon for **Firefox Web Browser**

1. In the address bar type [https://code.visualstudio.com](https://code.visualstudio.com) and press **Enter**

1. Click the green button labeled **.deb**

1. Leave **Open with** selected and click **OK**

1. In the **Ubuntu Software** window that appears, click the **Install** button

1. When installation is complete, click the icon for **Search your computer**

1. Type **Visual Studio Code**

1. In the search results, drag the **Visual Studio Code** icon to the **Launcher**, alongside the other icons that are already there. This will ensure you can find it each time you get back to coding.

1. Click the icon for **Visual Studio Code** to launch it

Everything we need is installed. Let’s set up the development environment.

You’ll want a clean working directory. The following steps are a strong recommendation, but if you already have a location or a specific process for structuring your code, you can skip them.

1. In Visual Studio Code, press Ctrl + Shift + E to bring up the **Explorer** panel

1. Click on the blue **Open Folder** button

1. Under **Places**, click on your username

1. Click the **Create Folder** button

1. Type **Development** and press **Enter**. You will automatically be brought into the **Development** folder. This folder will be the location that you store all your various development projects.

1. Once again, click the **Create Folder** button

1. Type **hello-world** and press **Enter**. You will automatically be brought into the **hello-world** folder.

1. Click the **OK** button

At this point, you have created a new folder for developing your first TypeScript project and you have it open in VS Code. Let’s continue setting up the development environment. To take full advantage of VS Code’s benefits, you’ll need to put it in **Explicit Project** mode. This is as simple as adding a configuration file.

1. In the **Explorer** panel, hover over the **HELLO-WORLD** folder

1. Click the icon for **New File**

1. Type **tsconfig.json** and press **Enter**. Your cursor will be automatically placed in the file for editing.

1. Type…

```
{
    "compilerOptions": {
      "target": "es5",
      "sourceMap": true
    }
}
```


As you start typing the above code, you’ll notice that IntelliSense is available to help you remember and choose available options.

This configuration file simply tells the compiler that, during building, the TypeScript code we write should be transpiled to ECMAScript 5 (the most widely-supported version of JavaScript) and to include a source map that will help any debuggers map the compiled code to our original TypeScript.

5. Press Ctrl + S to save the file

Setting up the compile task (Task Runner) in VS Code will make our lives easier. This way, we won’t have to leave VS Code to build the project.

1. Press **F1**

1. In the **Command Palette** that appears, type **Configure Task Runner** and press **Enter**

1. Arrow down to **TypeScript — tsconfig.json** and press **Enter**

The task options are automatically saved for us. The defaults will work fine.

With the editor set up, we can begin coding.

1. Create a new file called **hello.ts**

1. In it, type…

```
class speak {
    public static hello(person: string): string {
      return "Hello, " + person + ".";
    }
}

var user = "World";

console.log(speak.hello(user));
```


The code above creates a new [class](https://www.typescriptlang.org/docs/handbook/classes.html) called **speak**. The only thing in this class is a function called **hello**. This function accepts a single argument called **person**, which must be a string. The output is the string “Hello, ” plus the value of the **person** argument, plus a period at the end. We then set a variable** **called **user** with the value “World”. Finally, we log the returned value of speak.hello(user) to the console.

The result should be that “Hello, World.” is logged to the console.

Now, let’s build — or, transpile our TypeScript code to JavaScript.

Press Ctrl + Shift + B

After a moment, you will see two new files in the working folder. One of them is called **hello.js**. If you peek at the contents of this file, you’ll see that it is similar to our original TypeScript, but some functionality has been re-written to be compliant with ECMAScript 5. You’ll also notice that the last line references the source map (the other file that was created during the build process). This line and the source map itself are important to the debugging process.

This code will run in a browser or other common environment that runs JavaScript. In fact, we can test that now, right from within VS Code.

*Note: The remaining part of this post uses the built-in debugger in VS Code. This is really only good for back-end coding. The debugger runs Node behind the scenes. In the future, I may do some research into getting the debugger to run a headless browser, which would be much more useful for front-end coding.*

1. Press Ctrl+Shift+D to bring up the **Debug** panel

1. Click the icon for **Open launch.json**

1. Arrow down to **Node.js** and press **Enter**

By default, two configuration objects are written here. We are interested in the one named **Launch**.

1. Change the **program** property to **“${workspaceRoot}/hello.js”**. This will tell our debugger to start hello.js (our working transpiled TypeScript file) each time we launch it.

1. Change the **sourceMaps** line to **true**. This will tell the debugger that we have a source map that it should follow when reading out any errors.

1. Press Ctrl + S to save the configuration

Finally, press F5 to start debugging.

After a moment, the **DEBUG CONSOLE** should appear, and “Hello, World.” should be written to it.

Press Shift + F5 to stop debugging.

What happens when we introduce an error?

1. Press Ctrl + Shift + E to bring up the **Explorer** panel

1. Click on **hello.ts**

1. On the last line, remove a character from the call to **hello()**. Make it read something like **helo(…**

The first thing you will notice is that IntelliSense is already underlining that function call with a red squiggly line, indicating that something is wrong. If you hover over it, you’ll see the exact error.

We could correct the error now, but let’s see what happens when we try to build and launch the code.

1. Press Ctrl + S to save the file

1. Press Ctrl + Shift + B to build

1. Wait a moment for the build process to complete, then press F5

When we built the project, the TypeScript compiler is just fine with any errors; it just passes them along into the JavaScript file. However, when we pressed F5 and launched the debugger, it stopped, highlighting our error.

Press Shift + F5 to stop debugging, fix the error, build, and launch the debugger again. Once again, no errors. You can follow this pattern of editing, building, and launching the debugger as often as you like during coding sessions.

In the [next post](https://travishorn.com/building-upon-javascript-with-typescript-b2857451f505#.kqsyp6nz5), I walk though some of the basics of TypeScript that build upon JavaScript.