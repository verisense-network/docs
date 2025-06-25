
## Timer

Verisense provides a powerful timer module that enables developers to schedule delayed or recurring function executions. The module consists of:

* `#[init]`
* `#[timer]`
* `set_timer!()`

---

### `#[init]`: Initialization Hook

A Rust function decorated with the `#[init]` attribute macro serves as a special initialization handler. This function is automatically invoked when a new version of the WASM module is deployed or upgraded.

**Example:**

```rust
#[init]
pub fn timer_init() {
    storage::put(b"delay", format!("init").as_bytes());
}
```

In this example, `timer_init()` will be called automatically upon deployment of a new AVS WASM version, allowing you to perform any necessary initialization tasks.

---

### `set_timer!` and `#[timer]`: Scheduling Timers

The `set_timer!` macro is used to schedule a new timer that triggers a handler function after a specified delay. Its syntax is:

```rust
set_timer!(Duration, timer_handler(params));
```

#### Basic Usage

```rust
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

In this example:

* The `test_set_timer` function sets a timer that will trigger after 4 seconds.
* When the timer fires, the `test_delay` function is executed with the provided arguments.
* All timer handler functions must be decorated with the `#[timer]` attribute.

The `set_timer!` macro allows you to directly pass arguments to the timer handler, which are safely serialized and deserialized by the runtime.

---

### Implementing Intervals (Recurring Timers)

By default, `set_timer!` schedules one-shot timers. To implement periodic execution (intervals), you can schedule the next timer within the timer handler itself, effectively creating a recursive loop.

**Example:**

```rust
#[post]
pub fn test_set_timer() {
    set_timer!(std::time::Duration::from_secs(2), run_interval());
}

#[timer]
pub fn run_interval() {
    // Business logic executed on each interval
    storage::put(b"interval", b"running");

    // Schedule the next execution
    set_timer!(std::time::Duration::from_secs(1), run_interval());
}
```

In this pattern:

1. An initial timer is set to trigger after 2 seconds.
2. Inside `run_interval`, your business logic is executed.
3. At the end of each execution, a new timer is scheduled to run after 1 second.
4. This creates a continuous periodic execution loop.

This recursive approach allows you to implement interval-like behavior without native interval support.

---

### Summary

| Component      | Description                                                                  |
| -------------- | ---------------------------------------------------------------------------- |
| `#[init]`      | Automatically called on WASM deployment or upgrade for initialization tasks. |
| `set_timer!()` | Schedules a one-shot timer to invoke a handler after a specified delay.      |
| `#[timer]`     | Marks a function as a valid timer handler callable by the runtime.           |
| Intervals      | Achieved by recursively scheduling timers within timer handlers.             |
