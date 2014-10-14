Database & Web Form Security
==========================

To protect your database, you need Rack Utilities. These come prepackaged with Sinatra.

In your server.rb file or an application helper file, include these lines:

```shell
include Rack::Utils
alias_method :h, :escape_html 
```

The second line allows us to escape script input by simply putting "h" at the beginning of your embedded Ruby.

Like so:

```shell
<% @posts.reverse.each do |post| %>
  <%=h post.content %></p> 
<% end %> 
```

That's it! Now your form inputs are protected from malicious injection attacks.

Cleaning out your remote database
==============================

Heroku PostgresQL works the same way as PostgresQL locally.

To view a comprehensive list of commands you can perform on the PostgresQL addon, type:

```shell
heroku pg help  
```

To launch psql from your command line, type:

```shell
heroku pg:psql 
```

Once you are in psql, you can view the tables associated with your remote database using \dt. From there, use SELECT * from (table name) and DELETE from (table name) to manage entries manually.

You can view a list of all psql commands with \h. Exit postgresql with \q.



