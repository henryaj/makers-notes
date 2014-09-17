---
layout: page
title: Debugging
permalink: /debugging/
---
### Read the error message

When you encounter an error in testing, always read the error message carefully
– preferably read it aloud! Often, reading the error itself is enough to spot
and fix the bug.

### Look at the stack trace

If not, look through the stack trace that appears below the error message, which
will guide you through the lines of code that were executed when the error
appeared. Look at those lines carefully to try and spot the error, using the
error message and type to guide you.

If you've got a big string of classes linked up with `::`, only look at the last
one in the list when fixing the bugs.

### Run your code in `irb`

Run `irb` (the interactive Ruby prompt, or `pry` for those inclined) and
`require` your code files. From there, you can start creating instances of your
custom class objects and play with them – calling custom methods on them, seeing
what the outputs are, and where your errors might be appearing.

### Sanity checking values

If you can't see where a value is being created in a particular chunk of code,
try editing the code and entering a custom value to see if that value appears in
your error.

Commenting out lines of code to isolate the one that's broken is also a useful
technique.

### `.inspect`

Inside your RSpec test, you can add a `raise` method, and then use `inspect` on
an object to print out its details in your RSpec test log. This can be a very
powerful way of interrupting a method to see exactly what's going wrong and at
what point. Similarly, you can use `puts` inside your code to print out the
value of a variable or result of a method to inspect it.

A handy shorthand way of writing `puts` for debugging is just using `p`, which
is the equivalent of `puts an_object.inspect`. And saves you a whole three
letters to type.

### Big failures

Segmentation faults can be a pain to debug because they don't provide the stack
trace or any other useful data. Find the file the error refers to and comment
out the entire thing – then uncomment each method, one-by-one, and test to see
which error raises the segfault (or the similar 'stack level too deep' error).

Note it may be better to go through the test file and commment out the tests to
help identify which one is bringin the wrath of Seg Fault on you. If you're
lucky you'll even get it to the point where you can get a usable message like
`stack level too deep`, which is caused by an infinite loop somewhere (which
itself will cause the `segmentation fault` when your computer runs out of
memory).

### Code style

Indentation is important! Unlike in other languages, the Ruby interpreter
doesn't care about indentation. But a lack of proper indentation makes it much
harder to spot missing `do`s and `end`s and other syntax errors.

### Focus on a test with RSpec

Add the following line to your spec/spec_helper.rb file (you'll need to have
initialised it with `rspec --init` first):

```ruby filter_run :focus => true ```

This allows you to prepend a specific test with `fit` (instead of the usual
`it`) to focus in on it when running RSpec.

Make sure to change `fit` back to `it` when done, otherwise all of your tests
will be ignored.

### `xit` through the Gift Shop

And finally, if you just want to park that test and ignore it while you get on
with something else, change the `it` to an `xit` to make the test show as
pending when you run Rspec (making orange the new green).
