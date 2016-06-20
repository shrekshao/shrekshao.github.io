---
layout: post
title:  "VSCode Typings and Intellisense: Dummy Learning VS-Code 1"
comments: true
categories: Blog
tag: vscode
---

**Updated on Jun 20 2016 for 1.0 typings and 1.x.x VS Code**

I try to bring code intellisense to visual studio code for three.js today. The process is also suitable for other packages. 

<!--more-->

As it is said on Visual Studio Code website: 

> VS Code provides IntelliSense for built-in symbols of browsers, Node.js, and virtually all other environments through the use of type definition .d.ts files.
[DefinitelyTyped](https://github.com/DefinitelyTyped/DefinitelyTyped) is a repository of typings files for all major JavaScript libraries and environments.

We will use [Typings](https://github.com/typings/typings) to install these files. 

+ Make sure you've installed `node.js` and run:

    ```
    npm install typings --global
    ```
 
+ Go to your project directory, run: 

    ```
    typings init
    ```

    There will be a `typings.json` file generated. 
    
+ Now search for the `three.js` syntax file in `DefinitelyTyped`:

    ```
    typings search three
    ```

    It will show all matched results. Find the one we need, the name is `three`

    

+ Install `three`

    ```
    typings install three --save --global
    ```

    + If this doesn't work, try specify a domain for the typings, use this: 

    ```
    typings install dt~three --save --global
    ```

+ Now with 1.x.x VSCode, we need to generate a `jsconfig.json` file in the root of the project folder by clicking the light bulb button at the bottom right. 

    ![](/assets/blog-img/vscode-lightbulb.png)


That's it. Now we can enjoy the syntax intellisense in vscode!  For other languages and packages the process is similar. 

![](/assets/blog-img/threejs-intellisense.png)