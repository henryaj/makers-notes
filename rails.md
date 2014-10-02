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

Let's add a create method to our reviews controller.

`app/controllers/reviews_controller.rb`:

```ruby
def create
    @restaurant = Restaurant.find(params[:restaurant_id])
    @restaurant.reviews.create(params[:reviews].permit(:thoughts, :rating))
end
```

RSpec will now complain that we don't have an association between restaurants and reviews. Bummer. Time to fix that.

To `app/models/restaurant.rb`, add:

`has_many :reviews`

Time for a migration.

`$ rails g migration AddResturantIdToReviews restaurant:belongs_to`
`$ rake db:migrate`

This does some Rails magic – it interprets AddRestaurantIdToReviews and parses it, so it understands that it needs to add 'RestaurantId' to the Reviews model. Then, Rake runs the migration.

If you ever want to undo this, you can rollback a migration using

`$ rake db:rollback[n]`

where *n* is the number of migrations you want to roll back.

Now, if you look at your `schema.rb` you'll see the new assocation between restaurants and reviews.

RSpec now gives an error about a missing template for create, so time to create that. Let's add the following line to the end of the create method in the reviews model.

`app/controllers/reviews_controller.rb`:

```ruby
redirect_to restaurants_path
```

Finally, update your restaurants index.html.erb to display the actual reviews, which you can get at by calling `restaurants.reviews.each` and iterating over them.

Part 4
======

belongs_to
----------

a one line in the `review.rb` model will tie the review to a restaurant.

```ruby
belongs_to :restaurant
```

But - what if the restaurant gets deleted? This would lead to reviews with no
restaurant - orphan reviews. We'd want to KILL OUR ORPHANS. Note: do not kill
orphans.

over in restaurant.rb model
```ruby
has_many :reviews, dependent: :destroy
```
Validation
----------

> And if you ever need self validation
>
> Just meet me in the alley by the railway station
>
>
> -- Morrissey

So... we don't want two KFCs. We only want one. So we'll need some validations
to stop some fool creating too many.

First off, we go to the features... because we need a test!

```ruby
describe 'creating restaurants' do
    context 'a valid restaurant' do

        #our old tests are here

    context 'an invalid restaurant' do
        it 'does not let you submit a name that is too short' do
            visit '/restaurant'
            click_link 'Add a restaurant'
            fill_in 'Name', with: 'kf'
            click_button 'Create Restaurant'
            expect(page).not_to have_css 'h2', text: 'kf'
            expect(page).to have_content 'error'
        end
    end
end
```

The test will fail... so that's fun. Now - now we'll test the model with some
unit tests.

Some lucky people got some seperate folders and files in their spec directory
for `models` and `controllers` - but we can make them if they're missing, we're
not above a few `mkdir`.

Inside a `models` folder we make a `restaurant_spec.rb`

```ruby
require 'spec_helper'

RSpec.describe Restaurant, :type => :model do
    it 'is not valid with a name of less than three characters' do
        restaurant = Restaurant.new(name: "kf")
        expect(restaurant).not_to be_valid
    end
end
```

`:type => :model` gives your test methods appropriate for a model – you couldn't use Capybara here, for example.

Now this isn't an amzing test as it could `be_not` valid for lots of reasons.
But it's a good start, right? But we could specify exactly which error.

```ruby
require 'spec_helper'

RSpec.describe Restaurant, :type => :model do
    it 'is not valid with a name of less than three characters' do
        restaurant = Restaurant.new(name: "kf")
        expect(restaurant).to have(1).error_on(:name)
        expect(restaurant).not_to be_valid
    end
end
```

If we hit rspeci not it will say itdoesn't know what this `have` is about. So
let's add `rspec-collection_matchers` to the test group of our Gemfile, and run
bundle ot get them all installed.

Run rspec now it knows - and it's failing for reasonable reasons.

To pass this we should head over to our model - specifically `restaurant.rb`,
and add something like the following:

```ruby
validates :name, length: {minimum: 3}
```

Easy, eh? That will pass our model test, but not the original feature test.

Let's throw some more validations into our model test while we're here:

```ruby
it "is not valid unless it is a unique name" do
    Restaurant.create(name: "The Ivy")
    restaurant = Restaurant.new(name: "The Ivy")
    expect(restaurant).to have(1).error_on(:name)
end
```

Another fine failure is generated, which we can then go on to fix with

```ruby
validates :name, length: {minimum: 3}, uniqueness: true
```

Others are available - `format` for instance, or `presence` - do some reading to
find out more..

Let's implement this on the frontend and get the feature test passing.

So, where are we saving our restaurant? In the controller's `create` action.
Let's change that to something that changes behaviour depending on whether the
new restaurant is valid or not.

```ruby
def create
    @restaurant = Restaurant.new(params[:restaurant].permit(:name))
    if restaurant.save
        redirect_to restaurants_path
    else
        render 'new'
    end
end
```

`Restaurant.create` is a shorhand for `Restaurant.new.save`. By breaking the
steps up we get to do something new if the restaurant won't save - as it won't
do when it's invalid!

We're still failing - but at least we're not saving anything. Let's get some
errors up on our form and make a real mess of it

```erb
<% if @restaurant.errors.any? %>
    <div id="errors_explanation" >
        <h2> <%= plurazlize(@restaurant.errors.count, "error") %> prohibited this
        restaurant from being saved: </h2>
        <ul>
            <% @restaurant.errors.full_messages.each do |message| %>
                <li><%= message %></li>
            <% end %>
        </ul>
    </div>
<% end %>

<%= form_for @restaurant do |f| %>
    <%= f.label :name %>
    <%= f.input :name %>
    <%= f.submit %>
<%= end %>
```

Fun action on the `pluralize` helper method there. More on them tomorrow.

And that should pass the test.

We can play around with the message that comes through from the error

```ruby
validates :name, length: {minimum: 3, message: "Make name longer fool"}
```

For homework, stop evil restauranteurs from giving their reviews amazing and impossible ratings.. Create a review model spec and make sure that the rating field can only accept values from one to five. Hint: take a look at `inclusion:`.

## Helpers

Let's make a helper method that creates an average of all reviews. First, we test.

To `review_feature_spec.rb`, add the below. Here, we've extracted out the leaving review bits into a separate method.

```ruby
def leave_review(thoughts, rating)
    visit '/restaurants'
    click_link 'Review KFC'
    fill_in "Thoughts", with: thoughts
    select rating, from: 'Rating'
    click_button 'Leave Review'
end

it 'displays an average rating for all reviews' do
    leave_review('So so', "3")
    leave_review('Great', "5")
    expect(page).to have_content("Average: 4")
end
```

Now, in our `restaurant_spec.rb`, we add a unit test:

```ruby
...
describe '#average_rating' do
    context 'no reviews' do
        it 'returns "N/A" when there are no reviews' do
            restaurant = Restaurant.create(name: "The Ivy")
            expect(restaruant.average_rating).to eq 'N/A'
        end
    end
end
...
```

To make the test pass, we need to update our restaurant model.

`app/models/restaurant.rb`:

```ruby
def average_rating
    'N/A'
end
```

This passes the test so far, but obviously isn't very useful. Now we need a test in the case of having 1 review, and then one for when it has 2 or more reviews.

`restaurant_spec.rb`:

```ruby
...
context '1 review' do
    it 'returns that rating' do
        restaurant = Restaurant.create(name: "The Ivy")
        restaurant.reviews.create(rating: 4)
        expect(restaurant.average_rating).to eq 4
    end
end
...
```

Let's update our `average_rating` method.

`app/models/restaurant.rb`:

```ruby
def average_rating
    return 'N/A' if reviews.none?
    reviews.inject(0) {|memo, review| memo + review.rating}
end
```

And now for multiple reviews...

`restaurant_spec.rb`:

```ruby
...
context 'multiple reviews' do
    it 'returns the average' do
        restaurant = Restaurant.create(name: "The Ivy")
        restaurant.reviews.create(rating: 1)
        restaurant.reviews.create(rating: 5)
        expect(restaurant.average_rating).to eq 3
    end
end
...
```

And update our `average_rating` method...

`app/models/restaurant.rb`:

```ruby
def average_rating
    return 'N/A' if reviews.none?
    reviews.inject(0) {|memo, review| memo + review.rating} / reviews.count
end
```

(Note that without `#to_f` to convert an integer to a float, you would find you would only ever have whole number averages.)

BUT! Rails has a built-in method for this. We could instead have done:

```ruby
reviews.average(:rating)
```

Easy.

Now, add some code to the index page to display this average beside each restaurant.

```erb
<h3>Average rating: <%= restaurant.average_rating.to_i %></h3>
```

Looks a bit ugly - so let's get some stars in place.

Look up the unicode for a star  - it's: ★★★★☆

Let's change the feature spec to expect that output.

```ruby
it "displays an average rating for all reviews" do
    leave_review("so so", "3") # it's a string because that's what Capybara sees
    leave_review("Great!", "5")
    expect(page).to have_content("Average rating: ★★★★☆")
end
```

And now let's make sure the page is in utf-8 so we can use those good-lookin'
stars. Somewhere in the layout `<head>` section.

```erb
<meta charset="UTF-8">
```

Right, put together a helper method for generating those stars. It will live in
the `helpers` directory, inside a file called `reviews_helper.rb`, and under the module `ReviewsHelper`.

First we'll make a test for it - throw together a `helpers` folder in the spec
folder, and write a new test in a new file called `reviews_helper_spec.rb`.

```ruby
require 'rails-helper'

describe ReviewsHelper, :type => :helper do
    context "#star_rating" do
        it "does nothing for not a number" do
            expect(helper.star_rating('N/A')).to eq "N/A"
        end
    end
end
```

Time to write that helper method.

```ruby
module ReviewsHelper
    def star_rating(rating)
        rating
    end
end
```

Which passes. Refine with a new test:

```ruby
it "returns five black stars for five" do
    expect(helper.star_rating(5)). to eq '★★★★★'
end
```

Which needs an updated method to pass.

```ruby
module ReviewsHelper
    def star_rating(rating)
        return rating unless rating.is_a?(Fixnum)
        "★" * rating
    end
end
```

And it's time for another test:

```ruby
it "returns three black stars and two white stars for three" do
    expect(helper.star_rating(5)). to eq '★★★☆☆'
end
```

So we need to work out the remainder to put on the end

```ruby
module ReviewsHelper
    def star_rating(rating)
        return rating unless rating.is_a?(Fixnum)
        remainder = (5 - rating)
        "★" * rating + "☆" * remainder
    end
end
```

And now for the killer test:

```ruby
it "returns three black stars and two white stars for 3.5" do
    expect(helper.starr_rating(3.5)). to eq '★★★☆☆'
end
```

Alas - we'll get "3.5" back as 3.5 ain't a Fixnum. Let's do a bit of duck typing
rather than strict typing to identify the input as valid - does it respond to
`round`? That'll do it...

```ruby
module ReviewsHelper
    def star_rating(rating)
        return rating unless rating.respond_to?(:round)
        remainder = (5 - rating)
        "★" * rating.round + "☆" * remainder
    end
end
```

And that should pass those tests. So let's get back to our `index` to get the
feature test to finally pass.

```erb
<%= star_rating(restaurant.average_rating) %>
```

We created out own helper method. But Rails has a whole bunch more for us to
use, like `pluralize` and others!

Take a look at `ActionView::Helpers` in the docs.

Assignment
----------

Timestamp the review - and say how long ago it was posted. Something like...

`Great! posted 5 hours ago...`

AJAX
====

> “If you try to cure evil with evil
> you will add more pain to your fate."
>
> Sophocles, *Ajax*
"

First, set up yourself with the endorsements - get someone elses damn notes for
that.

Get hold of the `poltergeist` gem

```ruby
gem 'poltergeist'
```

in your Gemfile and then `bundle` the hell out of it.

You'll have to do a few config things to et this working - rails helper

```ruby
require 'capybara/poltergeist'
Capybara.javascript_driver = :poltergeist
```
and also
```ruby
config.use_transactional_fixtures = false
```

And then a shit load of stuff to do with DatabaseCleaner... see the link to
follow... along with the DatabaseCleaner gem itself.

So, now we're set up for JS tests - but we don't have any JS tests.

if we set `js: true` on the test we just built for endorsements, you'll see the
different driver load.

`application.js` - has MAGIC comments `//=`  - this determines the order in
which the JS loads on the page - the `//= require_tree .` is everything else in
the directory. (same for the `*=` css comments)

So, with the endorsements link in the index `endorsements-link` (we could give it a class to
make our lives easier)

so in the `endorsements.js/coffee` file...

```javascript
$(document).ready(function(){
    $('.endorsements-link').on('click', function(event){
        event.preventDefault();
        alert('event');
    })
})
```

This still refreshes if you're using `post` method in for the link on the `index` page. But check out that alert!

> Shift the JS in the layout to the after the content

```javascript
$(document).ready(function(){
    $('.endorsements-link').on('click', function(event){
        event.preventDefault();
        $.post(this.href);
    })
})
```

Now stick a JSON response over in the `EndorsementsController`

```ruby
render json: {new_endorsement_count: @review.endorsements.counr}
```

And now in the JS

```javascript
$(document).ready(function(){
    $('.endorsements-link').on('click', function(event){
        event.preventDefault();
        $.post(this.href, function(response){
        console.log(response)
        };
    })
})
```

And if we check the console we should see that number incrementing. So let's
change that number on the page!

Looking at the index, we could ass a `span` with a class `endorsements-class`
and add `<%= review.endorsements.count %>`, to allow us to target the number in
the JS.

```javascript
$(document).ready(function(){
    $('.endorsements-link').on('click', function(event){
        event.preventDefault();
        $.post(this.href, function(response){
            $('.endorsements_count').text(response.new_endorsements_count)
        };
    })
})
```

Which gets them all!

```javascript
$(document).ready(function(){
    $('.endorsements-link').on('click', function(event){
        var endorsementCount = $(this).siblings('.endorsements_count');
        event.preventDefault();
        $.post(this.href, function(response){
            endorsementCount.text(response.new_endorsements_count)
        };
    })
})
```

Which will only target the sibling of the link clicked to be updated **make sure
your `index` page is structured appropriately!**

And now your rspec will pass.

Homework
========

Get some pluralizing of endorsement working on the page. It'd be nice!!

## User authentication

### Devise

Devise is very well-documented – Google it.

To your Gemfile, add:

`gem 'devise`

and run

`$ rails generate devise:install`

then 

`$ rails generate devise User`

where User is whatever you want your user class to be called (but User is probably the safest bet for this). Remember, this is a model, so User is singular.

This generateds a model and a migration, and a custom route – `devise_for :users`. Type in `rake routes` to see all those custom Devise routes.

Have a look at `app/models/user.rb` to get the gist of your new User class.

Run `rake db:migrate` to the run the migration that Devise has given us. Now we're good to go!

In `views/layout/application.rb`, add a sign out link:

```erb
<% if user_signed_in? %>
<%= link_to "Sign out", destroy_user_session_path, method: :delete %>
...
```

And now we need sign in and sign up links, so add this to the above:

```erb
...
<% else %>
<%= link_to "Sign in", new_user_session_path %>
<%= link_to "Sign up", new_user_registration_path %>
```

All of this can be found in the Devise documentation and in `rake routes`.

### OmniAuth

You'll need to create a [Facebook Developer application](http://developers.facebook.com) and use those keys in the code above. For now, just replace the full strings – `ENV['FACEBOOK_KEY']` and `ENV['FACEBOOK_SECRET']` – with those values, but this is bad way of doing this.

The proper way of doing this is adding these values to the Rails `secrets.yml` file. They can then get called into the above code without needing to check them into version control. Call them using the following:

`Rails.application.secret.NAME_OF_SECRET`

Anyway – all of this is in [this tutorial](https://github.com/plataformatec/devise/wiki/OmniAuth:-Overview). Do it!

### Homework

Write tests and features for the following:

* You can't review a restaurant more than twice.
* You can't review your own restaurants.
* You can only delete your own restaurants.

## Uploading images with Paperclip

Install ImageMagick with

`$ brew install imagemagick`

and then add the Paperclip gem to your Gemfile:

`gem 'paperclip'`

and run `bundle install`.

Here's the boilerplate text from https://github.com/thoughtbot/paperclip:

```ruby
class User < ActiveRecord::Base
  has_attached_file :avatar, :styles => { :medium => "300x300>", :thumb => "100x100>" }, :default_url => "/images/:style/missing.png"
  validates_attachment_content_type :avatar, :content_type => /\Aimage\/.*\Z/
end
```

Add this into your Restaurant model, but obviously change `User` to `Restaurant`. Change `:avatar` to `:image`.

Now, we need a migration to add this :image to our restaurants. Generate it using

`$ rails generate paperclip restaurant image`

and then run `rake db:migrate` to add the new column to your database.

Have a look at `app/views/restaurant/new.html.erb`. Change the first line to the following – this lets you send larger files than normal:

`:html => { :multipart => true }`

and then add 

`<%= f.file_field :image %>`

to your form.

Go to your restaurants controller and add `:image` to your `.permit` statement.

Now, when a user creates a restaurant and includes an image, it gets saved to the /public directory.

In `views/restaurants/index.html.erb`, you now want to include a photo.

`<%= image_tag @restaurant.image.url(:thumb) %>`

### Homework

Figure out how to test this with capybara.