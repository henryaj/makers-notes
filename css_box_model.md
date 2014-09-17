---
layout: page
title: CSS and the Box Model
permalink: /css/
---

## Divs

Frequently, designers will mark out sections in a web page using `<div>`s. The problem with `<div>`s is that they aren't very descriptive. By definition, a `<div>` is a block element with no semantic meaning.

Much better is to use tags that are meaningful – which will be especially important for visually-impaired users that are using a screenreader.

Here are some better tags to use:

* `<section>` – a collection of thematically-grouped content.
* `<aside>` – content that's related to but not a part of the central content.
* `<footer>` – page footer.
* `<nav>` – navigation elements.

These tags are also more concise than using `<div class="...">` to identify each element on the page.

## CSS: The box model

This is a basic model for laying out CSS elements.

![Image of CSS box model](http://css-tricks.com/wp-content/csstricks-uploads/firebox.png)

The properties it uses are:

* **padding** – the space immediately around the content of the box
* **border** – immediately around the padding
* **margin** – immediately around the border
* **offset**

Using CSS feels like this:

![Peter Griffin tries CSS](http://i.imgur.com/Q3cUg29.gif)

## Useful resources
* [Learn CSS Layout](http://learnlayout.com) is an incredible resource.
* [Lean to Code HTML & CSS](http://learn.shayhowe.com/html-css/) is also very good.
* [CSS Tricks](http://css-tricks.com)
