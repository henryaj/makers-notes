---
layout: page
title: Inheritance and Composition
permalink: /inheritanceandcomposition/
---
## Which one to choose?

```
		Dog < Animal             Use inheritance for is-a.

		Van(BikeContainer)
		Garage(BikeContainer)    Use composition for has-a.
		Dock(BikeContainer)
```

A dog **is a***(n) animal, but a garage is not a bike container – it **has a** bike container. Thus, BikeContainer should not be a class but a *module* from which other classes can mix in using ```include```.