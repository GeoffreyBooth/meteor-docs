---
title: webapp
description: Documentation of Meteor's `webapp` package.
---

The `webapp` package is what lets your Meteor app serve content to a web
browser. It is included in the `meteor-base` set of packages that is
automatically added when you run `meteor create`. You can easily build a
Meteor app without it - for example if you wanted to make a command-line
tool that still used the Meteor package system and DDP.

This package also allows you to add handlers for HTTP requests.
This lets other services access your app's data through an HTTP API, allowing
it to easily interoperate with tools and frameworks that don't yet support DDP.

`webapp` exposes the [connect](https://github.com/senchalabs/connect) API for
handling requests through `WebApp.connectHandlers`.
Here's an example that will let you handle a specific URL:

```js
// Listen to incoming HTTP requests, can only be used on the server
WebApp.connectHandlers.use("/hello", function(req, res, next) {
  res.writeHead(200);
  res.end("Hello world from: " + Meteor.release);
});
```

`WebApp.connectHandlers.use([path], handler)` has two arguments:

**path** - an optional path field.
This handler will only be called on paths that match
this string. The match has to border on a `/` or a `.`. For example, `/hello`
will match `/hello/world` and `/hello.world`, but not `/hello_world`.

**handler** - this is a function that takes three arguments:

- **req** - a Node.js
[IncomingMessage](https://nodejs.org/api/http.html#http_class_http_incomingmessage)
object with some extra properties. This argument can be used to get information
about the incoming request.
- **res** - a Node.js
[ServerResponse](http://nodejs.org/api/http.html#http_class_http_serverresponse)
object. Use this to write data that should be sent in response to the
request, and call `res.end()` when you are done.
- **next** - a function. Calling this function will pass on the handling of
this request to the next relevant handler.
