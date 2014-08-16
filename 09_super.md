# Super

If you define a method of the same name as a parent's method, the child method takes priority.

```bikecontainer.rb```:

```ruby
module BikeContainer
	
	def dock(bike)
	@bikes << bike

end
```

```garage.rb```:

```ruby
class Garage

	include BikeContainer

	def dock(bike)
		bike.fix!
		@bikes << bike
	end

end
```

But there's a better way. Use ```super``` to call the parent's method. By doing this, you can define a custom child method (here, we want each Garage to fix the bike as well as docking it) without repeating yourself from the original method.

```ruby
class Garage

	include BikeContainer

	def dock(bike)
		bike.fix!
		super  # Runs the original dock method from BikeContainer
	end

end
```
