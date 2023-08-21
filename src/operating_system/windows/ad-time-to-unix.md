## Convert date/time attributes in Active Directory to UNIX time

The Active Directory stores date/time values as the number of 100-nanosecond intervals that have elapsed since the 0 hour on January 1, 1601 until the date/time that is being stored.

To convert AD date/time into UNIX time, we will need to convert the time from 100-nansecond intervals to seconds and then substruct the difference between UNIX epoch __(1970-01-01 00:00:00)__ and Windows epoch __(1601-01-01 00:00:00)__ which is 134,774 days __(or 11,644,473,600 seconds)__

```rust,edition2021
use anyhow::Result;
pub fn convert_windows_to_unix_time(windows_time: usize) -> Result<usize> {
    let time_diff = (windows_time / 10000000) - 11644473600;

    if time_diff < 0 {
        panic!("Windows time occurred before UNIX epoch");
    } else {
        Ok(time_diff)
    }
}

fn main() {
    assert_eq!(convert_windows_to_unix_time(133369301920000000).unwrap(), 1692456592);
}

```