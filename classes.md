# Classes

Think of classes as blueprints for Ruby objects – like Platonic forms; not the object itself but a model for it. Classes can be used to define methods and other properties that instances of that class then inherit when they are created (though they can later be overwritten).

When defining a class, note that the ```initialize``` method is called whenever an instance of a class is created.

Using ```self``` in a class method allows you to return the instance you are making.

An example class:


```ruby
class Superman
	def initialize
		@is_flying = false
	end
end

> Superman.new
=> <object_id: xxxxxxxxxxxx @is_flying: false>
```