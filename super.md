# Super

If you define a method of the same name as a parent's method, the child method takes priority.

```bikecontainer.rb``` :
```ruby
module BikeContainer
	
	def dock(bike)
	@bikes << bike

end
```

```garage.rb``` :
```ruby
class Garage

	include BikeContainer

	def dock(bike)
		bike.fix!
		@bikes << bike
	end

end
```

But there's a better way. Use `super` to call the parent's method. By doing this, you can define a custom child method (here, we want each Garage to fix the bike as well as docking it) without repeating yourself from the original method.

```ruby
class Garage

	include BikeContainer

	def dock(bike)
		bike.fix!
		super  # Runs the original dock method from BikeContainer
	end

end
```

You can also call arguments on `super`. Say you have a Ship class which takes an argument of the ship size:

```ruby
class Ship
  attr_reader: size

  def initialize(size)
    @size = size
  end
end
```

Now you can define the subclass, using `super` to call the parent method with a given argument.

```ruby
class Submarine < Ship
  def initialize
    super(2) # calls Ship's initialize method with arg of 2
  end
end
```

You could make this clearer by using a constant in the subclass:

```ruby
class Submarine < Ship
  SIZE = 3
  def initialize
    super SIZE
  end
end
```

You can also use `super` in string interpolation, like this:

```ruby
puts "#{super}"
```