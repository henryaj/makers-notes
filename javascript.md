# Javascript

A general-purpose, object-oriented programming language. There a number of key differences from Ruby: stricter syntax, no classes, very flexible.

Unlike other languages, however, it's the only language which can be run in the browser – on the client side (rather than the server side).

This allows for a number of advantages:

* dynamic page updates without needing to refresh
* increased speed and lower compute costs for the server

There are problems with it, too. All of a page's JS code is visible to, and editable by, the end user. So there are a number of processes that shouldn't happen client-side. As a result, JS tends to be used for UI enhancements on client-side.

Server-side JS implementations like Node.js are becoming more popular as well as they allow a development team to use the same language on both ends.

## Conventions

Variable names are in camel case, with a lowercase first letter. Curly braces are the equivalent of `do` and `end`.

`function` is a commonly-used keyword which takes the place of several keywords in Ruby:

// complete this section!
* `class`
* `def`

### Setting variables

```javascript
name = "hello"; // This is a global variable!
var name = "hello"; // That's better – an instance variable.

var name; // This creates a variable, like setting something to nil in Ruby.

// Create a bunch of variables in one go:
var player1, player2, game;
```

### No implicit returns

In Ruby, the last line of a method is returned by default. In Javascript, you have to specify a return.

```javascript
Game.prototype.winner = function() {
    return player1
}
```

### Specify your arguments!

```javascript
'abc'.toUpperCase    // raises an error
'abc'.toUpperCase()  // that's better!
```

### No `attr_writer` and `attr_reader`

By default in Javascript, all variables are readable and writable.

### Comparisons

JS uses `===` for comparisons.

## Some example code

Here we're defining Player and Game classes.

```javascript
function Player() {
    // code goes here
}

function Class() {
    // code goes here
}
```

Running Jasmine at this point gives some rather unhelpful errors, but the stack trace will help you out.

Another difference between Ruby and JS: in JS, you don't define methods inside a class. But you *do* definine initialise methods inside the class.

```javascript
function Player(name) {
    this.name = name;
    // Here we're passing in name as an argument and
    // initialising a player with a name.
}
```

```javascript
Game.prototype.winner = function() {

}
```

## Testing Javascript

By default, JS is tested in the browser. [Jasmine](http://jasmine.github.io) is a good BDD testing library for JS.