# Databases

> A digital washing machine for that gentleman! I'll send it by email
> —*[Enrique Comba Riepenhausen](http://ecomba.pro), 2014*

Think of a database as being like an Excel workbook. One workbook contains many sheets, each of which is a table and has a name. In each sheet, you have a number of columns, and a number of rows. Each row is a data point.  

Databases are very particular about the types of input they accept. There are no strings – there are only `VARCHAR`s (variable character fields) which usually have a limit of 255 characters. To store objects longer than this, you might have to use `TEXT` (in Postgres) or a `BLOB` (binary large object).

## Interacting with databases

Having to switch between your nice object-oriented language into SQL (structured query language) every five minutes is super annoying. So there's an intermediate layer, called ORM (Object Relational Mapping), that translates your OO commands into an SQL query.

This also saves you from having to know exactly how the database protocol works. Not all relational databases have the same protocol, but you can just use a different ORM to deal with this. (Note that this is a nice example of the *adapter pattern*.)

### ORMs – Datamapper

A tool like [Datamapper](http://datamapper.org) is an example of an ORM – it makes setting up and interacting with databases from within Ruby trivial.

For example, say you create the class `Link`:

```ruby 
class Link
  include DataMapper::Resource

  property :id,     Serial
  property :title,  String
  property :url,    String
end
```

Now, when you run `DataMapper.finalize`, it dives into the class you made, takes the class name, downcases it, pluralises it, turns it into a symbol, and makes a new table with that name. It also adds the `property` items as columns in the table.

Run `DataMapper.auto_upgrade!` and DataMapper will go and actually make the database and tables.

In your code, you can now make new links and do things to them.

```ruby
link = Link.create(title: 'Makers Academy',
                   description: 'Learn to code in 12 weeks',
                   url: 'http://www.makersacademy.com')

link.attributes (title: 'Join Makers Academy') # updates the row
link.save # saves the change

link.update (title: 'Join Makers Academy') # does the above but with one command

link.destroy # deletes the row
```

Other commands you can run:

* `x.new` – creates a new instance. Set its attributes with `x.attributes`
* `x.all` – returns the entire database
* `x.first` – returns the first element in the database
* `x.last` – returns the final element in the database (be careful with ordering, though, as this may not be done in a consistent way across databases)
* `x.get(3)` – returns the element with ID 3
* `x.first_or_create` – gets the first element if it exists, otherwise creates it

## Types of database

* **Relational** – store data and the relationships between data in many tables.
* **Key-value**