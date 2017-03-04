# JobScheduler
[![](https://img.shields.io/crates/v/job_scheduler.svg)](https://crates.io/crates/job_scheduler) [![](https://img.shields.io/crates/l/job_scheduler.svg)](https://raw.githubusercontent.com/lholden/job_scheduler/master/LICENSE)

A simple cron-like job scheduling library for Rust.

## Usage

Be sure to add the job_scheduler crate to your `Cargo.toml`:

```toml
[dependencies]
job_scheduler = "*"
```

Creating a schedule for a job is done using the `FromStr` impl for the
`Schedule` type of the [cron](https://github.com/zslayton/cron) library.

The scheduling format is as follows:

```text
sec   min   hour   day of month   month   day of week   year
*     *     *      *              *       *             *
```

Note that the year may be omitted.

Comma separated values such as `5,8,10` represent more than one time 
value. So for example, a schedule of `0 2,14,26 * * * *` would execute
on the 2nd, 14th, and 26th minute of every hour.

Ranges can be specified with a dash. A schedule of `0 0 * 5-10 * *` 
would execute once per hour but only on day 5 through 10 of the month.

Day of the week can be specified as an abbreviation or the full name.
A schedule of `0 0 6 * * Sun,Sat` would execute at 6am on Sunday and 
Saturday.

A simple usage example:

```rust
extern crate job_scheduler;
use job_scheduler::{JobScheduler, Job};
use std::time::Duration;

fn main() {
    let mut sched = JobScheduler::new();

    sched.add(Job::new("1/10 * * * * *".parse().unwrap(), || {
        println!("I get executed every 10 seconds!");
    }));

    loop {
        sched.tick();

        std::thread::sleep(Duration::from_millis(500));
    }
}
```

## Similar libraries

* [schedule-rs](https://github.com/mehcode/schedule-rs) is a similar rust library that implementing it's own cron syntax.
* [cron](https://github.com/zslayton/cron) is a cron expression parser for rust.
