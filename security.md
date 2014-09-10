# Security

Storing passwords in plain text (unencrypted) is a very bad idea for a number of reasons. Usually, passwords are stored as *hashes* in the database. 

To do this in Ruby, you can use the built-in BCrypt library.

```ruby
text_password = "s3cr3t"

BCrypt::Password:create(text_password)
=> "$2a$10$HD8iJKEzREKT28R9/D.4gux9ffJeflrDdPuXmN1MCXP7lbBkJY60."
```