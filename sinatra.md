# Sinatra

**Sinatra** is a DSL (*domain-specific language*, or a language written for a specific purpose) for building web applications.

```ruby
require 'sinatra'

get '/' do
    'hello Makers!'
end
```

This code will intercept an HTTP GET request sent to '/', and reply with 'hello Makers!'. Simple.