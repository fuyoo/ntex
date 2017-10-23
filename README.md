# Actix web [![Build Status](https://travis-ci.org/actix/actix-web.svg?branch=master)](https://travis-ci.org/actix/actix-web) [![Build status](https://ci.appveyor.com/api/projects/status/kkdb4yce7qhm5w85/branch/master?svg=true)](https://ci.appveyor.com/project/fafhrd91/actix-web-hdy9d/branch/master) [![codecov](https://codecov.io/gh/actix/actix-web/branch/master/graph/badge.svg)](https://codecov.io/gh/actix/actix-web) [![crates.io](http://meritbadge.herokuapp.com/actix-web)](https://crates.io/crates/actix-web)

Asynchronous web framework for [Actix](https://github.com/actix/actix).

* [API Documentation (Development)](http://actix.github.io/actix-web/actix_web/)
* [API Documentation (Releases)](https://docs.rs/actix-web/)
* Cargo package: [actix-http](https://crates.io/crates/actix-web)
* Minimum supported Rust version: 1.20 or later

---

Actix web is licensed under the [Apache-2.0 license](http://opensource.org/licenses/APACHE-2.0).

## Features

  * HTTP 1.1 and 1.0 support
  * Streaming and pipelining support
  * Keep-alive and slow requests support
  * [WebSockets support](https://actix.github.io/actix-web/actix_web/ws/index.html)
  * Configurable request routing
  * Multipart streams
  * Middlewares

## Usage

To use `actix-web`, add this to your `Cargo.toml`:

```toml
[dependencies]
actix-web = "0.1"
```

## Example

* [Basic](https://github.com/actix/actix-web/tree/master/examples/basic.rs)
* [Stateful](https://github.com/actix/actix-web/tree/master/examples/state.rs)
* [Mulitpart streams](https://github.com/actix/actix-web/tree/master/examples/multipart)
* [Simple websocket session](https://github.com/actix/actix-web/tree/master/examples/websocket.rs)
* [Tcp/Websocket chat](https://github.com/actix/actix-web/tree/master/examples/websocket-chat)


```rust
extern crate actix;
extern crate actix_web;
extern crate futures;

use actix::*;
use actix_web::*;

fn main() {
    let system = System::new("test");

    // start http server
    HttpServer::new(
        // create application
        Application::default("/")
            .resource("/", |r|
                r.handler(Method::GET, |req, payload, state| {
                    httpcodes::HTTPOk
                })
             )
             .finish())
        .serve::<_, ()>("127.0.0.1:8080").unwrap();

    // stop system
    Arbiter::handle().spawn_fn(|| {
        Arbiter::system().send(msgs::SystemExit(0));
        futures::future::ok(())
    });

    system.run();
}
```
