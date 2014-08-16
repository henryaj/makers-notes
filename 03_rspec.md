# RSpec

Short for 'Ruby specification'.

Benefits of testing:
* Your code works.
* You write less code! Specifically, you write only the code that's needed to pass your tests – nothing more.
* Trying to break your tests gives you a good idea of how robust your code is and what everything's doing.

## Getting started

Running ```rspec --init``` in the directory you want to test produces a ```lib``` folder with a ```spec_helper.rb``` file inside.

## Good practice

* Group your tests by context. This makes them must clearer to read when they're displayed when running RSpec.
* Mark a test as pending using ```xit``` instead of ```it```.