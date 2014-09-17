# HTML best practice

Always start with a doctype and lang.

```HTML
<!DOCTYPE html>
<lang='en'>
```

Specify the UTF-8 character set.

```HTML
<meta charset='UTF-8'>
```

Avoid using generic `<div>` tags and prefer more descriptive tags, such as

* `<section>`
* `<main>`
* `<article>`

Class names shouldn't refer to location or styling (like `left` and `right`) – prefer classes that describe the content of the element.

Avoid misusing header tags – they're for headers, not for styling text! Prefer `<strong>` or `<p>` with a class and then restyle it the way you want in your CSS.