# Databases

Think of a database as being like an Excel workbook. One workbook contains many sheets, each of which is a table and has a name. In each sheet, you have a number of columns, and a number of rows. Each row is a data point.  

Databases are very particular about the types of input they accept. There are no strings – there are only VARCHARs (variable character fields) which usually have a limit of 255 characters. To store objects longer than this, you might have to use TEXT (in Postgres) or a BLOB (binary large object).

## Types of database

* **Relational** – store data and the relationships between data in many tables.
* **Key-value**