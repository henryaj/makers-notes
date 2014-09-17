# AJAX

AJAX is a browser technology that allows pages to be dynamically changed. Think about commenting on Facebook – the entire page doesn't refresh, the comment just appears live.

AJAX allows a browser to make background HTTP requests and then use those resources to modify the page in some way.

Here, we're preventing a form from refreshing the page when we hit enter or click Submit on a form.

```js
$(document).ready(function(){
    $('#a_form').on('submit', function(event){
        event.preventDefault();
```

Now, we want to grab the input from our form and do something with it. Here, we're searching for a user with the GitHub API.

```js
        var url = "https://api.github.com/users/" + $('#username').val();
        $.get(url);
```

(Note that `$('#username').val()` gets the value that's been typed into the username field.)

We have to modify the last line to make it *do* something with the data from the GET request:

```js
        $.get(url, function(user){
                $('<article>Repos: ' + user.public_repos + '</article>').appendTo('.profile-container');
```

We can append this to add a failure mode in case an invalid user is searched for:

```js
        }).fail(function(){
            alert('Could not find this user.');
            })
        })
```

You can empty forms the same way you read from them – but by passing in an argument of an empty string:

```js
        }).always(function({
            $.('username').val('');
            }))
```
