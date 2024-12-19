
## Timer

Verisense has a powerful timer module. This module consists of 

- #[init]
- #[timer]
- set_timer!()


### `set_timer!` and `#[timer]`

The macro `set_timer!` is used to set a new timer, this timer will be triggered in the `delay` time. Its signature is as follows:

```
set_timer!(Duration, timer_handler(params));
```

let's look at how to use it. 

```
#[post]
pub fn test_set_timer() {
    storage::put(b"delay", format!("init").as_bytes());

    let a = "abc".to_string();
    let b = 123;
    set_timer!(std::time::Duration::from_secs(4), test_delay(a, b));
}

#[timer]
pub fn test_delay(a: String, b: i32) {
    storage::put(b"delay", format!("delay_complete {} {}", a, b).as_bytes()).unwrap();
}

```

In the above example, in the post function `test_set_timer`, we use a `set_timer!` to create a new timer, this timer will be
triggered after 4 seconds, the triggered function is `test_delay`. You can write parameters of the handler in the `set_timer!` 
macro directly.

Let's inspect the timer handler further. The handler function `test_delay` must be decorated by `#[timer]`, which assigns the 
decorated function the timer handler role. `test_delay` will be called after 4 seconds when `test_set_timer` is called.

So the `set_timer!` is just a delay function, how to implement intervals?

**Interval** means executing a function periodically. We can use `set_timer!` with recursive call to implement it. For example:

```
#[post]
pub fn test_set_timer() {
    set_timer!(std::time::Duration::from_secs(2), run_interval());
}

#[timer]
pub fn run_interval(){
    // do something
    set_timer!(std::time::Duration::from_secs(1), run_interval());
}
```

In this example, we set a timer which would be triggered in 2 seconds. While `run_interval` was triggered, you can do business in 
`run_interval()`, and at the last line of this function, just set a new timer, which would be executed in 1 second, and then triggered
the same `run_interval()` by tail recursion. Then later the program will run forever by intervals of 1 second. We implement intervals by this way.

### `#[init]`

A rust function decorated by this attribue macro is a special timer handler, it will be triggered when a new version of wasm code is upgraded.

For example:

```
#[init]
pub fn timer_init() {
    storage::put(b"delay", format!("init").as_bytes());
}
```

The `timer_init()` will be called automatically when a new version of one AVS wasm code is upgraded in Verisense.

You can refer to a more complex example at [here](https://github.com/verisense-network/verisense/blob/main/nucleus-examples/examples/timer.rs#L3).


