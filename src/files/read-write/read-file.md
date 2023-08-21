## Read entire file content into a variable 

Write a three-line message to a file, then reads the entire file content into a variable.

### Rust

```rust,edition2021,no_run
use anyhow::Result;
use std::fs::File;
use std::io::prelude::*;

fn main() -> Result<()> {
    let path = "lines.txt";

    let mut file = File::create(path)?;
    file.write_all(b"Hello world!")?;
    Ok(())
}
```
