# Collections

## Ranges

```ruby
(1..5) # => 1, 2, 3, 4, 5

(1...5) # => 1, 2, 3, 4
```

You can use letters or numbers in a range.

The items in the range **must increment**. If you want the items to descend rather than ascend, make the range as usual and then call ```reverse``` on it.

## Arrays

Store pieces of data in a list that can be retrievesd by number (or rather, by *index*).

A quick way of filling an array with strings:

```ruby
%w(hello there) # => ["hello", "there"]
```

```ruby
array.each 		# => Loops over each item in an array
array.map 		# => Does something to each item in an array, and creates a new array of the same size from the results
array.map!		# => Like map, but overwrites the original array with the result of the process
array.select 	# => Evaluates a condition and returns matching items from the array
array.reject 	# => Does the opposite of select!
```

## Hashes

These store *key-value pairs*. Keys **must** be unique. In other languages, hashes are called *dictionaries*.

Values can be retrieved from a hash by specifying a key.

```ruby
hash = Hash.new
hash = {:name => "Henry", :city => "London", :age => 150}
hash[:city] # => returns "London"
```

As of Ruby 1.9, hashes have a fixed order.
