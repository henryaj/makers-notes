# Setter and Getter methods

```ruby
class Person

	def initialize(name, age)
		@name, @age = name, age
	end

	def name 					# This is a getter method.
		@name
	end

	def name=(name)		# This is a setter method.
		@name = name
	end

end
```

Instead of the above setter and getter methods, you can use ```attr_```:

```ruby
class Person


	attr_reader(:name)
	attr_writer(:name)

	# Or, to do both:

	attr_accessor(:name)

	# Now the initialize method:

	def initialize(name, age)
		@name, @age = name, age
	end

end
```

This saves a lot of typing!