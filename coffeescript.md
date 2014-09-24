# [CoffeeScript](http://coffeescript.org)

A Ruby-like language that compiles into JavaScript.

CoffeeScript is well-documented, succinct, and elegant.

## Worked example – FizzBuzz

This assumes you've installed Mocha, CoffeeScript and Chai with the following commands from the folder you're working in:

```shell
npm install coffee-script
npm install chai
npm install mocha
```

You can also use the `-g` flag when running the above commands to install these packages globally, rather than locally.

If you're using Mocha for tests, you need to 'register' a CoffeeScript compiler with Mocha first.

```shell
mocha --compilers coffee:coffee-script/register 
```

You might also have to create a Mocha options file – /test/mocha.opts – and add the following line:

`--compilers coffee:coffee-script/register`

Set up your folder structure like this:

```
├── src
│   └── fizzbuzz.coffee
└── test
    ├── fizzbuzz_spec.coffee
    └── mocha.opts
```

Now, let's write a test.

`test/fizzbuzz_test.coffee`:

```coffee
chai = require 'chai'
expect = chai.expect
Fizzbuzz = require '../src/fizzbuzz'

describe 'Fizzbuzz', -> 
  it '3 is divisible by 3', ->
    fizzbuzz = new Fizzbuzz()
    expect(fizzbuzz.isDivisibleByThree(3)).to.be.true
  
  it '1 is not divisible by 3', ->
    fizzbuzz = new Fizzbuzz()
    expect(fizzbuzz.isDivisibleByThree(1)).to.be.false

```

(Note the capital 'F' in Fizzbuzz, because we're declaring a class.)

Be careful of that third line above, though – if you have a fizzbuzz.js file it will use that instead of your fizzbuzz.coffee file!

And now, the code:

`src/fizzbuzz.coffee`:

```coffee
class Fizzbuzz
  isDivisibleByThree: (number) ->
    number % 3 == 0

module.exports = Fizzbuzz
```

And so on... you know how FizzBuzz goes.

Here are our tests after a little bit of refactoring – note the `before` method:

```coffee
chai = require 'chai'
expect = chai.expect
Fizzbuzz = require '../src/fizzbuzz'

describe 'Fizzbuzz', -> 
  before ->
    @fizzbuzz = new Fizzbuzz()

  it '3 is divisible by 3', ->
    expect(fizzbuzz.isDivisibleByThree(3)).to.be.true
  
  it '1 is not divisible by 3', ->
    expect(fizzbuzz.isDivisibleByThree(1)).to.be.false
```

Note the @ sign in '@fizzbuzz' in the before function. This is like `this.fizzbuzz` in JavaScript – it creates the variable before assigning it.