# Debugging

### Read the error message

When you encounter an error in testing, always read the error message carefully – preferably read it aloud! Often, reading the error itself is enough to spot and fix the bug.

### Look at the stack trace

If not, look through the stack trace that appears below the error message, which will guide you through the lines of code that were executed when the error appeared. Look at those lines carefully to try and spot the error, using the error message and type to guide you.

### Run your code in `irb`

Run `irb` (the interactive Ruby prompt) and `require` your code files. From there, you can start creating instances of your custom class objects and play with them – calling custom methods on them, seeing what the outputs are, and where your errors might be appearing.

### Sanity checking values

If you can't see where a value is being created in a particular chunk of code, try editing the code and entering a custom value to see if that value appears in your error.

Commenting out lines of code to isolate the one that's broken is also a useful technique.

### `.inspect`

Inside your RSpec test, you can add a `raise` method, and then use `inspect` on an object to print out its details in your RSpec test log. This can be a very powerful way of interrupting a method to see exactly what's going wrong and at what point. Similarly, you can use `puts` inside your code to print out the value of a variable or result of a method to inspect it.

### Big failures 

Segmentation faults can be a pain to debug because they don't provide the stack trace or any other useful data. Find the file the error refers to and comment out the entire thing – then uncomment each method, one-by-one, and test to see which error raises the segfault (or the similar 'stack level too deep' error).

### Code style

Indentation is important! Unlike in other languages, the Ruby interpreter doesn't care about indentation. But a lack of proper indentation makes it much harder to spot missing `do`s and `end`s and other syntax errors.

### Focus on a test with RSpec

Add the following line to your spec/spec_helper.rb file (you'll need to have initialised it with `rspec --init` first):

```ruby
filter_run :focus => true
```

This allows you to prepend a specific test with `fit` (instead of the usual `it`) to focus in on it when running RSpec.

Make sure to change `fit` back to `it` when done, otherwise all of your tests will be ignored.
