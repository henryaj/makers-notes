# Design Patterns

Just as there are repeated problems in architecture and engineering, so there are similar problems in programming. Design patterns, in computer science, are set ways of solving progamming problems.

* Decorator pattern
* Factory method
* Builder pattern
* Adapter pattern
* Visitor pattern

## Decorator methods

## Factory methods

## Builder methods

These are methods that initialize the objects you want to create.

You wouldn't start out using this method – ideally, the pattern will emerge *as you are coding*.

## Adapter methods

Adapters are a class that handle a request from a client, and contain a method that will turn a request from a client into something that the adaptee can accept.

Much like a two-pin to three-pin converter, these classes convert input messages into a format appropriate for the adaptee.

```ruby
def first_character(string)
  # returns the first char of a string
  string.chr
end

def adapter(number)
  # converts a number into a string so the
  # method above can use it
  number.to_s
end

puts first_character(adapter(490283)) => 4
```

## Visitor methods

Adds functionality that draws information from existing classes without changing the classes themselves.

Examples:

* extracting the age from an object of class `Employee`
* extracting an object's price to calculate VAT

Importantly, a visitor class doesn't affect the core functions of any other classes – it follows the *open/closed principle*. They're easier to comprehend and adapt, and improve code readability over using blocks.