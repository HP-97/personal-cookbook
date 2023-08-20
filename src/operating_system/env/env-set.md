# Set an environmental variable 

TODO:
```rust,edition2021
use std::env;

let key = "KEY";
env::set_var(key, "VALUE");
assert_eq!(env::var(key), Ok("VALUE".to_string()));
```