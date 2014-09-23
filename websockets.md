# WebSockets
* unlike HTTP, where you have the request/response cycle, WebSockets allow you to do **two-way, real-time communication between browser and server**.
* opens a two-way 'pipe' between client and server through which data can move
* no need for client to request data from the server -- it gets pushed through instantly
* is a new protocol, independent of HTTP. Just like HTTP has `http://`, WebSockets has `ws://`
* works over port 80 like HTTP, so usually isn't affected by firewalls and proxies

## How do you use it?
* various ways of implementing it
* could use WebSockets directly, but this is complex and not all browsers support the protocol as it's relatively new (ratified in 2011)

### socket.io
* a JavaScript library for node like [socket.io](http://socket.io) is ideal
* this kind of thing is *made* for node.js – you need to keep lots and lots of sockets open but usually not doing very much, so the ability to have multiple threads and non-blocking IO is essential
* socket.io has two parts:

> * a server-side library that works with node.js, and
> * a local client-side library for the browser that talks to the server

* it aims to work on every platform, browser, and device
* primarily uses WebSockets (or a version of WebSockets, which is why you need to use the socket.io library on the client side)
* BUT! can fall back on older real-time methods like [long polling](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling), where an HTTP request is open and kept open until it's needed
* install it using `npm install socket.io`

### Other implementations
* other implementations exist like [Pusher](http://pusher.com), which allow you to send requests to a third-party server which pushes them to your clients for you (this gets around restrictions some web hosts have on WebSockets, like Heroku)





