# Databases

> A digital washing machine for that gentleman! I'll send it by email
> —*[Enrique Comba Riepenhausen](http://ecomba.pro), 2014*

Think of a database as being like an Excel workbook. One workbook contains many sheets, each of which is a table and has a name. In each sheet, you have a number of columns, and a number of rows. Each row is a data point.  

Databases are very particular about the types of input they accept. There are no strings – there are only `VARCHAR`s (variable character fields) which usually have a limit of 255 characters. To store objects longer than this, you might have to use `TEXT` (in Postgres) or a `BLOB` (binary large object).

## Types of database

* **Relational** – store data and the relationships between data in many tables.
* **Key-value**

## Interacting with databases

Having to switch between your nice object-oriented language into SQL (structured query language) every five minutes is super annoying. So there's an intermediate layer, called ORM (Object Relational Mapping), that translates your OO commands into an SQL query.

This also saves you from having to know exactly how the database protocol works. Not all relational databases have the same protocol, but you can just use a different ORM to deal with this. (Note that this is a nice example of the *adapter pattern*.)