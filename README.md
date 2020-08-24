Router Os
================
Mikrotik API for Rust

**This is a fork of [Mikrotik-rust crate by woowe](https://github.com/woowe/Mikrotik-rust). login has been fixed to work post-v6.43: [Mikrotik API links](https://wiki.mikrotik.com/wiki/Manual:API#Initial_login)**

*TODO: support both old and new login methods.*

This API gives you the ability to connect to your mikrotik router over a tcp connection.

[![Build Status](https://travis-ci.org/Wooowe/Mikrotik-rust.svg?branch=master)](https://travis-ci.org/Wooowe/Mikrotik-rust) [![Latest Version](https://img.shields.io/badge/crates.io-0.0.21-orange.svg)](https://crates.io/crates/routeros_rust)

### Installation

Add router_os via your `Cargo.toml`:
```toml
[dependencies]
routeros_rust = "*"
```

#### or

```toml
[dependencies.routeros_rust]
git = "https://github.com/Wooowe/mikrotik-rust"
```

### Usage
```rs
extern crate router_os;

use router_os::ApiRos;
use std::net::TcpStream;

fn main() {
    let mut stream = TcpStream::connect("192.168.1.1:8728").unwrap();

    let mut apiros = ApiRos::new(&mut stream);
    apiros.login("admin".to_string(), "".to_string());

    apiros.write_sentence(vec!["/ip/address/print".to_string()]);

    loop {
        let reply = apiros.read_sentence();

        if reply.len() == 0 {
            continue;
        }

        if reply[0] == "!done" {
            break;
        }
    }
}
```
