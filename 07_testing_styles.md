# Testing styles

## Chicago style TDD

Bring in all the objects and classes you need for testing as you need them.

Say you're modelling a bike hire scheme. You might start with the ```Bike``` class and quickly realise you need to manipulate a ```DockingStation``` – so you'd start building a ```DockingStation``` class as you come to it.

## London style TDD

Use doubles to isolate the unit tests from each other. When your ```Bike``` needs a ```DockingStation```, just use a double (like a 'ghost object' whose functionality and expected responses you define in advance) when running your tests.

An example of this:

```ruby
it 'can rent a bike from a station' do
	old_street = double :station
	bike       = double :bike

	expect(old_street).to receive(:release_a_bike).and_return(bike)

	# Note that the location of this line within the method is very important!

	person.rent_bike_from old_street

end
```