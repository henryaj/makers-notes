---
layout: page
title: Yield
permalink: /yield/
---

Yield is a keyword that allows you to execute a block of code passed to a method.

```ruby
def hello
  puts "Hello " + yield
end

hello do
  "David"
end

# returns "Hello David"
```

Yield can also be passed arguments:

```ruby
def hello(arg)
  puts "Hello " + yield(arg)
end

hello("Dear") do |arg|
  "#{arg} David"
end

# returns "Hello Dear David"
```
