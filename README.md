# Deadpool

Deadpool is a dead simple async pool for connections and objects
of any type.

## Features

Feature             | Description
------------------- | --------------------------------------
`postgres`          | Enable support for `tokio-postgres` connection pooling. This feature also includes a `statement` cache.

## Example (using `postgres` feature)

```rust
use std::env;

use deadpool::Pool;
use deadpool::postgres::Manager;
use tokio_postgres::Config;

#[tokio::main]
fn main() {
    let mut cfg = Config::new();
    cfg.host("/var/run/postgresql");
    cfg.user(env::var("USER").unwrap().as_str());
    cfg.dbname("deadpool");
    let mgr = Manager::new(cfg tokio_postgres::NoTls);
    let pool = Pool::new(mgr, 16);
    loop {
        let mut client = pool.get().await.unwrap();
        let stmt = client.prepare("SELECT random()").await.unwrap();
        let rows = client.query(&stmt, &[]).await.unwrap();
        let value: f64 = rows[0].get(0);
        println!("{}", value);
    }
}
```

## License

Licensed under either of

 * Apache License, Version 2.0 ([LICENSE-APACHE](LICENSE-APACHE) or http://www.apache.org/licenses/LICENSE-2.0)
 * MIT license ([LICENSE-MIT](LICENSE-MIT) or http://opensource.org/licenses/MIT)

at your option.
