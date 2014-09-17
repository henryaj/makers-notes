# Logic and booleans

All of

```ruby
<
>
==
>=
<=
```

evaluate to `True or False`.

The fact that a variable is set makes it `True`. Everything in Ruby is `True` unless it's `False` or `nil`.

## Ternary operators

```ruby
<condition> ? <command_if_true> : <command_if_false>
```

Evaluates the condition and returns one of the two answers depending on its truthiness. A nice one-line truth test.

## If, Elsif, Unless

```ruby
if ...
	# code
elsif ...
	# code
end
```

```ruby
while ...
	#code
end
```

```ruby
until
	#code
end
```

Avoid the use of `!` to negate a statement – use `unless` instead.
