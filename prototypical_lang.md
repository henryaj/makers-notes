# Prototypical languages

## The rules

* There are no classes or inheritances.
* Everything is an object, even methods.
* Objects have 'slots'. Slots contain objects, including method objects.
* A message can return a value in a slot, or invoke a method.
* If an object is unable to respond to a particular message, it will send it up the chain to its prototype (its 'parent').

## Trying it out – [IO](http://iolanguage.org/)

`$ brew install io`

and then

`$ io`

will take you to the IO REPL.

### Writing commands

When writing commands, the command goes on the *right* of the *receiver*.

`"Welcome to io, Makers" println`

Here, `println` is the command, and the string we're printing is the receiver.

### Prototypes

`Bike := Object clone`

This will clone the Object prototype into Bike.

`Bike description := "A beautiful Fuji Feather`

This writes the string to the bike's 'description' slot.

You can also write methods into a slot:

`Calculator add := method(a, b, a + b)`

Now you can run `Calculator add(1,3)` and get back 4.