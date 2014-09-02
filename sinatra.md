# Sinatra

**Sinatra** is a DSL (*domain-specific language*, or a language written for a specific purpose) for building web applications.

```ruby
require 'sinatra'

get '/' do
    'hello Makers!'
end
```

This code will intercept an HTTP GET request sent to '/', and reply with 'hello Makers!' in plain text. Simple.

## Routes

In the above example, '/' is the *route*. You can define a bunch of other routes, too. For example:

```ruby
get '/hi' do
    <h1>Hello Makers!</h1>
end
```

Now, accessing the website path `/hi` will yield the above message (which in this case will appear big and bold on the page, as we have wrapped it in `h1` tags.)

## Testing with Cucumber

[Cucumber](http://cukes.info) is used for *acceptance testing* – it acts as if it were a person using the application to make sure it works. You can use Cucumber with Sinatra by using the [cucumber-sinatra gem](https://github.com/bernd/cucumber-sinatra).

Get started by running:

```shell
$ cucumber-sinatra init --app SampleApp lib/sample_app.rb
...
$ rackup
```

Run your Cucumber tests from the command line with `$ cucumber`.

Now, let's describe a feature using Cucumber:

```shell
$ mkdir feature
$ touch homepage.feature
```

```cucumber
Feature: The homepage
  In order to show off Cucumber at Makers
  I want to present Cucumber
```

This will run, but doesn't have any actual tests (or *scenarios*). Let's add some in the same file.

```cucumber
  Scenario: Being greeted
    Given I am having a good day
    When I am on the homepage
    Then I should see "Hello Makers!"
```

This `Given`/`When`/`Then` notation is particular to Cucumber (or specifically Gherkin, the language Cucumber is written in).

For each step, you write a step definition in Ruby. 

cucumber-sinatra has some built in step definitions, so you can write some steps without needing to define them (like `When I am on the homepage`). These built-in definitions live in `features/step_definitions/`, and there are support files in `support/` which do things like substituting 'homepage' for '/'. **Consider deleting `features/step_definitions/web_steps.rb` to get some practice in writing your own step definitions.**

Cucumber also uses a language called [Capybara](https://github.com/jnicklas/capybara).