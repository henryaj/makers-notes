# Design Patterns

Just as there are repeated problems in architecture and engineering, so there are similar problems in programming. Design patterns, in computer science, are set ways of solving progamming problems.

* Decorator pattern
* Factory method
* Builder pattern
* Adapter pattern
* Visitor pattern

## Decorator methods

## Factory methods

## Builder pattern

A builder pattern defines an instance for creating an object but lets subclasses decide which class to isntantiate. Builder patterns are used for building these complex objects independently of the mechanisms that go into creating their components. They allow the complex object to be easily 'customised' by specifying the number and types of components required.

Builder patterns may be desired to simplify the inputs required to create the object (e.g. avoid having to manually build arrays needed as arguments for component objects) or to hide the detail of an object's creation from other applications/users. 

You wouldn't start designing your code using a builder pattern. Rather, it emerges in response to growing complexity. Generally designs starts out by using the factory method to manage creation of components and evolves towards a builder pattern as the number of required components increases.

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