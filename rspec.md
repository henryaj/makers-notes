# RSpec

Short for 'Ruby specification'.

Benefits of testing:
* Your code works.
* You write less code! Specifically, you write only the code that's needed to pass your tests – nothing more.
* Trying to break your tests gives you a good idea of how robust your code is and what everything's doing.

## Getting started

Running ```rspec --init``` in the directory you want to test produces a ```lib``` folder with a ```spec_helper.rb``` file inside.

## Raising errors

```ruby
expect(lambda {...} ).to raise_error
# or:
expect{ ... }.to raise_error
```

If you don't use curly braces, the error kills the RSpec ```expect``` statement. The braces effectively allow RSpec to rescue the test from the error.

## Helper methods

Methods you define in RSpec to assist you with tests.

```ruby
def helper
	20.times{ dock.fill(bike) }
end
```

Now you can call ```helper``` in other tests.

## Testing modules

Use 'shared examples'.

## Good practice

* Group your tests by context. This makes them must clearer to read when they're displayed when running RSpec.
* Mark a test as pending using ```xit``` instead of ```it```.
* Avoid "magic numbers", like ```20.times.do```. What does the 20 signify?
