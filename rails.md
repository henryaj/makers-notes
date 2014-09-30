# Rails

Rails is a popular web framework. Sinatra is a DSL, not a framework – it gives you much more flexibility and fewer conventions when you're building a website. Rails is, well... like being on rails. There are fewer decisions for you to make, and more decisions that are made for you.

## Getting started

`$ gem install rails` will install the Rails gem. Expect it to take a while. `rails --help` gives a nice help menu.

Make a new Rails app:

`$ rails new yelp_clone -d postgresql -T`

The `-T` switch turns off the built-in Rails test suite. `-d` preconfigures your app for a particular type of database – in this case, PostgreSQL.

## The folders

So many folders... This is what they all do:

* `vendors` – a place for resources that you haven't written but are needed for the project, like JQuery.
* `public` – public resources. These will remain available even if the server goes down. Includes all your error pages by default.
* `log` – keeps server logs and terminal output.
* `config` – configuration information, including `database.yml` which includes database configuration details, a routes file, 
* `bin` – contains your specified version of Rails.
* `app` – where the magic happens. Contains models, views and controllers.

## Get it going

### Boot the server

Start up the server using `rails server` or `rails s` and visit http://localhost:3000. You'll likely need to run a `rake` task to get your database going – visit the page and you'll be told which.

`$ bin/rake db:create`

You may need to append RAILS_ENV=test to get your tests working.

### Add some testing gems

Now, add some gems to your Gemfile!

```
gem 'rspec-rails', group: :test
gem 'capybara', group: :test
```

With these, we also want to run this command:

`$ bin/rails generate rspec:install`

This gets RSpec going by creating a /spec directory and a helper file.

In your spec/rails_helper.rb file, add the line:

`require 'capybara/rails'`

This lets you use Capybara in your testing environment.

### The first test – home page with a link

Make a spec/features/ directory, and make a new spec file inside it.

`restaurants_feature_spec.rb`:

```ruby
require 'rails_helper'

describe 'restaurants' do
    context 'no restaurants have been added' do
        it 'should display a prompt to add a restaurant' do
            visit '/restaurants'
            expect(page).to have_content 'No restaurants'
            expect(page).to have_link 'Add a restaurant'
        end
    end
end
```

Now run `rspec`, which will say that there's no route matching `/restaurants`. Simple.

The config/routes.rb file has lots of clues as to how to write routes – have a look at them.

`routes.db`:

```ruby
resources:restaurants
```

If you now run `rake routes` you'll get a list of the different routes that this has created.

Running `rspec` again, we get another `RoutingError` – this time, there's no Restaurants controller. Time to make one!

`$ bin/rails g controller restaurants`

(Here, 'g' is short for generate.)

Now, RSpec gives us a different error – that there's no action /index for restaurants. Let's fix that.

`app/controllers/restaurants_controller.rb`:

```ruby
class RestaurantsController < ApplicationController

def index
end

end
```

Now we get a different error – that /app/views/ is missing an index view.

`$ touch app/views/restaurants/index.html.erb`

(Note the requirement to use the double file extension here.)

Now our error is that there's no text on the page! Fix it:

`app/views/restaurants/index.html.erb`:

```html
No restaurants yet!
```

Cool – but now RSpec is telling us we need a link on the page.

`app/views/restaurants/index.html.erb`:

```html
No restaurants yet!
<a href='#'>Add a restaurant</a>
```

And now, our test is passing.

### The second test – creating a restaurant

Add the following to `spec/features/restaurants_feature_spec.rb`:

```ruby
context 'restaurants have been added' do
    before do
        Restaurant.create(name: 'KFC')
    end

    it 'should display restaurants' do
        visit '/restaurants'
        expect(page).to have_content('KFC')
        expect(page).not_to have_content('No restaurants yet') 
    end
end
```

Now we need a Restaurants model to satisfy our failing test.

`$ bin/rails g model restaurant name:string description:text`

This command will add 'name' and 'description' properties to the database for each restaurant, and make a migration file that you can run to create these properties. Each item gets an ID automatically. Note that 'restaurant' here is singular, but the controller refers to 'restaurants'.

If you make a mistake, you can type the above command but using 'rails d' – for destroy – to remove the migrate.

Then:

`$ bin/rake db:migrate`

which will run all of your database migrations.

(A word on migrations – don't go into those files and edit them. If you want to remove database tables or change the schema, instead write another migration that does that.)

Now, in `restaurants_controller.rb` we want to get all of those restaurants from the database. Let's add a method for that (*the below replaces the old method*):

```ruby
def index
    @restaurants = Restaurant.all
end
```

And in `app/views/restaurants/index.html.erb`:

```erb
<% if @restaurants.any? %>
    <% @restaurants.each do |restaurant| %>
        <h2> <%= restaurant.name %> </h2>
    <% end %>
<% else %>
    No restaurants yet
<% end %>

<a href='#'>Add a restaurant</a>
```

## Migrations – adding a column to a database

```shell
$ rails g migration AddDescriptionToRestaurants description:text
$ rake db:migrate
```

## Associations

Let's add some reviews for our restaurants.

`app/spec/features/review_spec.rb`:

```ruby
require 'rails_helper'

describe 'reviewing' do
    before do
        Restaurant.create(name: 'KFC')
    end

    it 'allows users to leave a review using a form' do
       visit '/restaurants'
       click_link 'Review KFC'
       fill_in "Thoughts", with: "so so"
       select '3', from: 'Rating'
       click_button 'Leave Review'
       
       expect(current_path).to eq '/restaurants'
       expect(page).to have_content('so so')
    end

end
```

First, we need a new route for reviews. Update `routes.rb` to have a nested resource:

```ruby
resource :restaurants do
    resource :reviews
end
```

Then, add a link (using Rails' `link_to` helper) to `new_restaurant_review_path` (you can see this path appearing in `rake routes`).

Now we need a new controller.

`$ rails g controller reviews`

In `app/controllers/reviews_controller.rb`, add the 'new' method:

```rb
def new
    @restaurant = Restaurant.find(params[:restaurant_id])
    @review = Review.new
end
```

This sets up @restaurant and @review which get passed into the 'new review' form in the next step.

Keep following the errors RSpec is giving you. Now we need a view:

`app/views/reviews/new.html.erb`:

```erb
<%= form_for [@restaurant, @review] do |f| %>
<%= f.label :thoughts %>
<%= f.text_area :thoughts %>

<%= f.label :rating %>
<%= f.select :rating, (1..5) %>
<%= f.submit 'Leave review' %>
<% end %>
```

Cool. Now we need a model for reviews – currently they aren't being stored in the database!

`$ rails g model review thoughts:text rating:integer`