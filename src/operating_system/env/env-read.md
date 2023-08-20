## Read an environment variable 

Fetches the environment variable `key` from the current process

### Rust

Source: https://doc.rust-lang.org/std/env/fn.var_os.html

```rust,edition2021
use std::env;

let key = "HOME";

match env::var(key) {
    Ok(val) => println!("{key}: {val:?}"),
    Err(e) => println!("couldn't interpret {key}: {e}")
}
```


### Python

Source: https://docs.python.org/3/library/stdtypes.html#mapping-types-dict

```python
import os

def read_env_variable(key: str) -> str | None:
    return os.environ.get(key)

if __name__ == "__main__":
    key = "HOME"

    if (val := read_env_variable(key)):
        print(f"{key}: {val}")
    else:
        print(f"was not able to find defined env variable: {key})
```