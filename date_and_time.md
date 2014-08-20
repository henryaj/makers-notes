# Date and Time

Date and time are painful to work with. Ruby's built-in ```Time``` and ```Date``` classes help.

## Time

```ruby
Time.now
# => 2014-08-20 15:33:47 +0100 
```

## Date

```ruby
require 'date'
DateTime.new
# => #<DateTime: -4712-01-01T00:00:00+00:00 ((0j,0s,0n),+0s,2299161j)> 

DateTime.now.asctime
# => "Wed Aug 20 15:39:31 2014"
```

## Testing time

How do you test a method which requires a wait (such as charging the user when renting a Boris bike and returning it half an hour later)?

Use [timecop](https://github.com/travisjeffery/timecop).

First, install timecop with

```ruby
gem install timecop
```

Then require timecop in your code.

```ruby
require 'Timecop'
```

Now you're ready to use it to manipulate time. For example, in your test you can write 

```ruby
Timecop.freeze(Time.local(2014))
```

Now, every time you (or your code) calls ```Time.now```, it will return

```ruby
=> 2014-01-01 00:00:00 +0000
```

In the Boris bike example, you can use ```Timecop.travel``` in your test to jump forward in time (which saves you having to wait in real-time for your method to run).

```ruby
hire(bike)
Timecop.travel(1801) # travels forward by half an hour and one second
dock(bike)
=> "Ride was over 30 mins; you need to pay £2.50"
```
