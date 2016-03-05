---
layout: post
comments: true
categories: Blog
tag: vscode
---

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
    
+ Now search for the `three.js` syntax file in `DefinitelyTyped`, use `--ambient` to include files from `DefinitelyTyped`:

    ```
    typings search three --ambient
    ```

    It will show all matched results. Find the one we need, the name is `three`

    

+ Install `three`

    ```
    typings install three --save --ambient
    ```

That's it. Now we can enjoy the syntax intellisense in vscode!  For other languages and packages the process is similar. 

![](/assets/blog-img/threejs-intellisense.png)