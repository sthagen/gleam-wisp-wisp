![Wisp](https://github.com/lpil/wisp/blob/main/docs/images/cover.png?raw=true)

[![Package Version](https://img.shields.io/hexpm/v/wisp)](https://hex.pm/packages/wisp)
[![Hex Docs](https://img.shields.io/badge/hex-docs-ffaff3)](https://hexdocs.pm/wisp/)

Wisp is a practical Gleam web framework for rapid development and easy maintenance.
We worry about the hassle of web development, and you focus on writing your
application.

It is based around two concepts: handlers and middleware.

# Handlers

A handler is a function that takes a HTTP request and returns a HTTP
response. A handler may also take other arguments, such as a "context" type
defined in your application which may hold other state such as a database
connection or user session.

```gleam
import wisp.{type Request, type Response}

pub type Context {
  Context(secret: String)
}

pub fn handle_request(request: Request, context: Context) -> Response {
  wisp.ok()
}
```

# Middleware

A middleware is a function that takes a response returning function as its
last argument, and itself returns a response. As with handlers both
middleware and the functions they take as an argument may take other
arguments.

Middleware can be applied in a handler with Gleam's `use` syntax. Here the
`log_request` middleware is used to log a message for each HTTP request
handled, and the `serve_static` middleware is used to serve static files
such as images and CSS.

```gleam
import wisp.{type Request, type Response}

pub fn handle_request(request: Request) -> Response {
  use <- wisp.log_request(request)
  use <- wisp.serve_static(request, under: "/static", from: "/public")
  wisp.ok()
}
```

# Learning Wisp

The [Wisp examples](https://github.com/gleam-wisp/wisp/tree/main/examples)
are a good place to start. They cover various scenarios and include comments and
tests.

API documentation is available on [HexDocs](https://hexdocs.pm/wisp/).

# Wisp applications

These open source Wisp applications may be useful examples.

- [https://packages.gleam.run/](https://github.com/gleam-lang/packages): A HTML
  serving application that uses an SQLite + LiteFS database, deployed to Fly.io.
