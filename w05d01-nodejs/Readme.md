# NodeJS, npm and ExpressJS

## Learning Competencies
By the end of this chapter, you should be able to:
- Create custom modules with `node`
- Compare and contrast how `node` finds built-in modules vs. custom modules
- Import and use core modules like `fs`, `path`, and `process`
- Learn about NPM & install packages using NPM
- Explain how a `package.json` file works
- Explain why we add the `node_modules` folder to a `.gitignore` file

## Overview

### NodeJS Introduction
Node.js is a server-side platform built on Google Chrome's JavaScript Engine (V8 Engine). Node.js was developed by Ryan Dahl in 2009 and its latest version is v0.10.36. The definition of Node.js as supplied by its official documentation is as follows −

    Node.js is a platform built on Chrome's JavaScript runtime for easily building fast and scalable network applications. Node.js uses an event-driven, non-blocking I/O model that makes it lightweight and efficient, perfect for data-intensive real-time applications that run across distributed devices.

Node.js is an open source, cross-platform runtime environment for developing server-side and networking applications. Node.js applications are written in JavaScript, and can be run within the Node.js runtime on OS X, Microsoft Windows, and Linux.

Node.js also provides a rich library of various JavaScript modules which simplifies the development of web applications using Node.js to a great extent.

```Node.js = Runtime Environment + JavaScript Library```


You can install node.js [here](https://nodejs.org/en/). There also exists a tool called `nvm`, which you can use to manage different versions of node, but for now let's install the LTS version of Node on the website.

You can read more about installing node on Windows [here](http://blog.teamtreehouse.com/install-node-js-npm-windows)

### what wikipedia says??
Node.js is an open-source, cross-platform JavaScript run-time environment for executing JavaScript code server-side. Historically, JavaScript was used primarily for client-side scripting, in which scripts written in JavaScript are embedded in a webpage's HTML, to be run client-side by a JavaScript engine in the user's web browser. Node.js enables JavaScript to be used for server-side scripting, and runs scripts server-side to produce dynamic web page content before the page is sent to the user's web browser. Consequently, Node.js has become one of the foundational elements of the "JavaScript everywhere" paradigm, allowing web application development to unify around a single programming language, rather than rely on a different language for writing server side scripts.
### Why's and How's ??

#### What is a Module in Node.js?
- Consider modules to be the same as JavaScript libraries.

- A set of functions you want to include in your application.

For instance :

You learnt how to make an API call using AJAX with XHR,
then AJAX with JQuery, well, it can be done much easier using ``request`` NodeJS module, [see](https://www.npmjs.com/package/request) for yourself.

#### Meet "Hello world"
This is your first code in Nodejs. In the terminal, create a file called `first.js` and add the following text:
```js
console.log('Hello World!');
```

#### `require` and `module.exports`
Node.js comes with built-in support for modules. These are not the same as ES2015 modules, these are called `common.js` modules and have a syntax that makes use of the `require`, `module` and `exports` keywords. Modules allow us to write code in one file and export that code to another. Since we are not using `script` tags (as we are not in the browser), we need a way of loading JavaScript from one file to another, and modules are our answer.

Let's take a look at some examples using Node.js modules. Create a file called `helpers.js`, which will export some data to another file called `main.js`. In our `helpers.js`, let's add the following:
```js
module.exports = {
    name: "Elie",
    sayHi(){
        console.log(`Hi ${this.name}!`);
    }
}
```
Now in our `main.js`, let's add the following:
```js
var helpers = require('./helpers');
helpers.sayHi();
```
If we run `node main.js` we should see the text "Hi Elie!" in the console. So how have we done this? We used the `module.exports` object to send an object to another file (`main.js`). To import the module we use the `require` keyword followed by the name of a module. Notice that we are using a **relative** path here. This is **very** important, because if we do not use a relative path, Node will try to find a module that either comes with Node.js or that we have installed externally. When you are using your own modules, **always** use relative paths.


### Built-in modules
So far we have seen how to create our own modules, but there are quite a few modules that come built into Node.js. We call these modules `core` modules and many of them are the foundation for more complex technologies and libraries that we will be using. You can see a full list of them [here](https://nodejs.org/dist/latest-v6.x/docs/api/). Let's examine some of the built-in modules as we will be using a few of them later!

#### File System

The file system or `fs` module can be used to read files, write to files, and append files. It is extremely useful for working with text files, comma separated values (csv) files and many others. The `fs` module can also be used to create and modify files and folders.

Let's first create a file called `data.txt` and place some text inside of it. Then, create a file called `read.js` and place the following code in it:
```js
var fs = require("fs");

fs.readFile('data.txt', function(err,data){
    console.log(data.toString());
})
```
When we run `node read.js` we will see the contents of that file. Notice that the second parameter passed to `readFile` is a callback function! We will discuss a little bit later why that is, but keep this example in the back of your mind.

#### Asynchronous environment
What do you notice about the code above? The `readFile` function accepted a filename and then a callback function! What we are seeing is that the entire Node.js environment is asynchronous. When one process begins running, it does not block the rest of our code from continuing to execute. While this makes Node.js very effective for certain tasks, it also makes it tricky to manage when first learning it. We will see how to manage our asynchronous code using callbacks and promises, but it's essential to remember that Node.js uses the event loop to handle processes asynchronously.

You can read more about this [here](http://stackoverflow.com/questions/17607280/why-is-node-js-asynchronous) and [here](https://blog.risingstack.com/node-hero-async-programming-in-node-js/).

#### Path

The path module is quite helpful for working with file paths, extending file paths, and determining types of paths.
```js
var path = require("path");

// Normalize a path
console.log(path.normalize('/test/test1//2slashes/1slash/tab/..')); // /test/test1/2slashes/1slash

// Join multiple paths together
console.log(path.join('/first', 'second', 'something', 'then', '..')); // /first/second/something

// Resolve a path (find the absolute path)
console.log(path.resolve('first.js'));

// Find the extention of a filename
console.log(path.extname('main.js')); // .js
```
#### Process

The process module can be used for quite a few things, including accepting command line arguments (`argv`) and listening for specific events (`on`). You can listen for a variety of events, including exiting or even the [next tick](https://nodejs.org/dist/latest-v6.x/docs/api/process.html#process_process_argv) of the event loop. We can also use the `process` object to access environment variables using `process.env` Instead of importing this module, `process` is a reserved keyword in Node that we can use! Let's make a file called `args.js` and inside let's place the following:
```js
console.log(process.argv);
```
Now let's run `node args.js 1 2 3` and we should see an array with values! The first value refers to the command which we are using to run `node`, the second is the absolute path to our file and the remaining ones are our arguments. How can we now print out only a list of `1`, `2` and `3`?

#### HTTP

The HTTP (and HTTPS) module allows you to create a server and issue HTTP requests and responses. You can use this module on its own to create a server, but using it involves a fair amount of code and complexity. This module is one of the essential parts of `express.js`, which is a framework that abstracts some of the more challenging and tedious parts of routing.
```js
var http = require('http');
// notice below, the first parameter to createServer is a callback function!
var server = http.createServer(function(req, res, next) {
  // sending some header info in my response
  res.writeHead(200, {'Content-type' : 'text/html'});
  // send some data to the client
  res.write("<h1>Hello World!</h1>");
  // end the response
  res.end();
});
// notice below, another callback function!
server.listen(3000, function(){
  console.log("Go to localhost:3000");
});
```
To practice with core node modules and learn more, you can walk through [learnyounode](https://github.com/workshopper/learnyounode), which is a wonderful resource for learning node.js fundamentals.

You can also see a small app using the `fs` module [here](https://github.com/rithmschool/node_curriculum_examples/tree/master/core_modules)




## npm
When you install `node`, you also get access to a package manager that you can use to install *external node modules*, or modules that do not come built in with Node.js. This package manager is called `npm` and we will be using it to install packages. To manage these packages, `npm` uses a special file called a `package.json` that stores information about the packages we have installed.

**Note** - there is another package manager called `yarn`, which is gaining popularity, but we will be using `npm` for this tutorial. You can read more about `yarn` [here](https://yarnpkg.com/).

### What wikipedia says??
> npm is automatically included when Node.js is installed. npm consists of a command line client that interacts with a remote registry. It allows users to consume and distribute JavaScript modules that are available on the registry. Packages on the registry are in CommonJS format and include a metadata file in JSON format. Over 477,000 packages are available on the main npm registry. The registry has no vetting process for submission, which means that packages found there can be low quality, insecure, or malicious. Instead, npm relies on user reports to take down packages if they violate policies by being insecure, malicious or low quality. npm exposes statistics including number of downloads and number of depending packages to assist developers in judging the quality of packages.

### Why's and How's??
#### `package.json`
To create a `package.json` file we can run the command `npm init`. This will begin the creation process. As part of the process you will be prompted for a fair amount of information about your project; to accept the default answers provided by `npm`, just keep hitting enter. We will discuss in more detail what these options are, but for now you can simply keep pressing enter until the file is created. To create a `package.json` without having to keep pressing enter you can simply run `npm init --yes` or `npm init -y`

#### Installing external modules
To install an external module we use the `npm install NAME_OF_PACKAGE` command. It's essential that when installing modules, we add the name of the module and version number to our `package.json` file. This will help other developers install and use the same packages and help us remember which packages we installed. We could manually type these into our package.json, but thankfully there is an easier way. When installing a package make sure to add the `--save` flag after.

Let's install an external module called `request`. This module is very useful for making server side requests (to other APIs). To do this we will first run the following commands to create a folder, `package.json` and essential files. This will all be run in terminal.
```
mkdir external_modules
cd external_modules
npm init --y # create a package.json without pressing enter over and over
npm install --save request # install the request module
touch main.js
```
Inside of our `main.js`, let's add the following.
```js
var request = require("request");

request('http://swapi.co/api/people/1', function(error, response, body) {
    if (!error && response.statusCode == 200) {
        console.log(JSON.parse(body)); // Show the JSON for the Star Wars Character
    }
});
```
Now if we run `node main.js` we should see some JSON data about Luke Skywalker!

#### Ignoring `node_modules`
When we install the `request` module, a folder gets created in the root directory of our application called `node_modules`. If you look inside of here, you will actually see every single module that the `request` module depends on (we call these 'dependencies'). Since this file is quite large, when we push our code on GitHub, we do not want the `node_modules` folder to be there. To avoid git tracking this folder, we add the text 'node_modules' to a `.gitignore` file. The terminal command to do this is `echo node_modules > .gitignore.`

You may be wondering, what happens if someone forks and clones our repository? How do they know which packages to install from npm? This is **exactly** why it is so important to add a `--save` every time that you `npm install`. If a `package.json` contains the dependencies for a project, you can simply run `npm install` and `npm` will search through the `package.json` file for all dependencies and install them.


## Exploration
- You check other modules and their documentations [here](https://www.npmjs.com/)
