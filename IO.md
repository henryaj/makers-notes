---
layout: page
title: IO
permalink: /io/
---

```ruby
some_file = File.new('newfile.txt','w')
some_file.puts "Hello!"
some_file.close
```

`File` is a Ruby subclass of `IO` which handles all input and output to Ruby programs. The above script will write a message to a file, and then close the file.

Instead of 'w', you can use the 'a' flag to append to a file:

```ruby
some_file = File.new('file_to_append_to.txt','a')
```

You can read from the file using `File.open` and `readlines.each`:

```ruby
some_file = File.open('new_file.txt')
some_file.readlines.each do |line|
  puts lines
end
some_file.close
```

These methods are useful but don't tend to get used in webapps – databases are preferred because their IO is non-blocking.

## CSV files

```ruby
require 'csv'

CSV.foreach("new.csv") do |row|
  puts row.inspect
end
```

The above script loads up the CSV library, and then opens `new.csv` and extracts each row as an array.