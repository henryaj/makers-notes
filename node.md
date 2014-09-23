# node.js

Node is a web application framework written in JavaScript.

Getting started can be as simple as serving up a .js file by running `node` at the command line.

In the node world, you have **npm** instead of RubyGems, Node Packaged Modules instead of Gems, and npm modules are installed locally by default, not globally. 

More likely, you'll want to use a generator like [Yeoman](http://yeoman.io/) and a framework like [express.js](http://expressjs.com/) to get started.

## Components

* **npm** is a package manager for installing packages, which are a bit like Gems. **`npm` assumes you want to install packages locally, rather than globally – `gem` behaves in the opposite way.**
* **package.json** in the root of your project keep track of all of your dependencies for your project, like a Gemfile.
* **Express.js** or similar frameworks is a little like Rails or Sinatra – it's a web framework that lets you create web apps in a more streamlined way by giving you everything you need to get started.
* **Yeoman** (or `yo`, no relation to the awful smartphone app) is a system for generating a skeleton project, based on node and your chosen framework. That's why you need the Express generator when running `yo express`.
* **Bower** lets you install web components (like JQuery, Bootstrap...) really quickly.
* **Grunt** is like `make` or `rake` – it's a task runner for JavaScript.
* **Mocha** and **Zombie** are testing libraries.

## Hints
* When testing an image click in Zombie, you need to use `browser.evaluate()` in your test. Inside the brackets should be some Javascript that you want to run, like a bit of JQuery.