## Read entire file content into a variable 

Write a three-line message to a file, then reads the entire file content into a variable.

```rust,edition2021,no_run
use std::fs::File;

fn main() -> Result<(), Error> {
    let path = "lines.txt"

    let mut file = File::create(path)?;
    file.write_all(b"Hello world!")?;
    Ok(())
}
```