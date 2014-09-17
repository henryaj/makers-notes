---
layout: page
title: Blocks, Procs and Lambdas
permalink: /blocksprocslambdas/
---

* *Blocks* get used every day. All languages have them.
* *Procs* and *lambdas* are useful, but less common. They aren't featured in all languages. Very handy for Rails.

## Blocks

```ruby
phrase = "The rain in spain falls mainly on the plain"

# string.gsub(regex_matcher, replace_with)

phrase.gsub(/[aeiou]/, "*")

# => "Th* r**n *n sp**n f*lls m**nly *n th* pl**n"
```

Instead of providing an argument like this, you can pass in a piece of code to execute (a *block*):

```ruby
phrase.gsub(/[aeiou]/) { |vowel| vowel.upcase }

# => "ThE rAIn In spAIn fAlls mAInly On thE plAIn"
```

The bit in curly braces is the block.

Blocks are flexible with the number of arguments they take. For example, ```each_with_index``` expects 2 variables, but you could pass in 1 or 0.

Blocks are efficient in terms of both speed and memory. But blocks can't be referred to elsewhere in your code – use them once and they're done.

## Procs

```ruby
crazy_upcaser = Proc.new do |word|
	puts word.upcase.reverse.gsub(/[aeiou]/, "*")
end
```

To call the Proc, you can pass it as an argument with an ampersand:

```ruby
['cat', 'dog', 'horse'].each(&crazy_upcaser)

=>	TAC
		GOD
		ESROH
```

You can only pass one block to a method, but you can pass in multiple procs! You can also pass in Procs as an argument to a method.

```ruby
def sing_a_song(verse, chorus)
	verse.call
	verse.call
	verse.call
	chorus.call
	verse.call
	verse.call
	chorus.call
end

sing_a_song(Proc.new { puts 'la la la' }, 
						Proc.new { puts 'dum de dum' })
```

## Lambdas

```ruby
say_hello_proc 			= Proc.new { puts "Hello" }

```
```ruby
say_hello_lambda 		= lambda { puts "hello "}
```

Lambdas are very similar to Procs but they are *strict* with the number of arguments they take. This can be useful in some cases.

```ruby
round 1
	return "Batman"
	return "Superman"
end
# Batman wins.

round 2
	lambda { return "Batman" }.call
	return "Superman"
end
# Superman wins.
```

In round 2, the return statement is contained within a lambda so doesn't exit out of the method.

However, a Proc *does* exit out of the method:

```ruby
round 3
	Proc.new { return "Batman" }.call
	return "Superman"
end
# Batman wins.
```

Another way of defining lambdas is to use ```->```:

```ruby
 -> { return "Batman" }.call
```
