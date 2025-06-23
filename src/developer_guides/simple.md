# Creating a Simple Nucleus

In this chapter, we will guide you step-by-step through building a simple Nucleus (AVS).

Let's begin by setting up the code structure directly:

### Step 1: Create a New Rust Project

Navigate to your desired project directory and create a new Rust project using `cargo`:

```bash
cargo new --lib hello-avs
cd hello-avs
```

### Step 2: Update the `Cargo.toml` File

Replace the contents of your `Cargo.toml` with the following:

```toml
[package]
name = "hello-avs"
version = "0.1.0"
edition = "2021"

[lib]
crate-type = ["cdylib"]

[dependencies]
vrs-core-sdk = { git = "https://github.com/verisense-network/vrs-core-sdk.git", rev="1a822394029bc7bb36cbe6e09d10d2f57700b2c8" }
parity-scale-codec = { version = "3.6", features = ["derive"] }
```

### Step 3: Implement the Core Logic

Edit the file `src/lib.rs` and insert the following code:

```rust
use vrs_core_sdk::codec::{Decode, Encode};
use vrs_core_sdk::export;
use vrs_core_sdk::{get, post, storage};

#[derive(Debug, Decode, Encode)]
#[export]
pub struct User {
    pub id: u64,
    pub name: String,
}

#[post]
pub fn add_user(user: User) -> Result<u64, String> {
    let key = [&b"user:"[..], &user.id.to_be_bytes()[..]].concat();
    storage::put(&key, &user.encode()).map_err(|e| e.to_string())?;
    Ok(user.id)
}

#[get]
pub fn get_user(id: u64) -> Result<Option<User>, String> {
    let key = [&b"user:"[..], &id.to_be_bytes()[..]].concat();
    let result = storage::get(&key).map_err(|e| e.to_string())?;
    let user = result.map(|data| User::decode(&mut &data[..]).unwrap());
    Ok(user)
}

```

### Step 4: Compile the Project to WebAssembly

Finally, compile your project to WebAssembly by running the following command in your project's root directory (`hello-avs`):

```bash
cargo build --release --target wasm32-unknown-unknown
```

Upon successful compilation, you'll see a message similar to:

```
Finished `release` profile [optimized] target(s) in 0.09s
```

You have successfully created a simple Nucleus (AVS). The compiled WebAssembly executable (`hello_avs.wasm`) will be available in the directory `target/wasm32-unknown-unknown/release`.

## What Does `hello-avs` Do?

Let's break down the functionality provided by this simple Nucleus step-by-step:

First, a struct named `User` is defined:

```rust
#[derive(Debug, Decode, Encode)]
pub struct User {
    pub id: u64,
    pub name: String,
}
```

Nucleus interfaces are exposed externally using macros such as `#[post]` and `#[get]`.

* The `#[post]` macro is used for interfaces that modify the blockchain's storage, as modifications consume gas.
* The `#[get]` macro is used for interfaces that do not modify the storage.

The storage interaction APIs are as follows:

* `storage::put` writes data to storage.
* `storage::get` reads data from storage.

### Add a New User with an ID

```rust
#[post]
pub fn add_user(user: User) -> Result<u64, String> {
    // Construct the user's key
    let key = [&b"user:"[..], &user.id.to_be_bytes()[..]].concat();
    // Write data to storage
    storage::put(&key, &user.encode()).map_err(|e| e.to_string())?;
    Ok(user.id)
}
```

### Retrieve User by ID

```rust
#[get]
pub fn get_user(id: u64) -> Result<Option<User>, String> {
    // Construct the user's key
    let key = [&b"user:"[..], &id.to_be_bytes()[..]].concat();
    // Retrieve data from storage
    let result = storage::get(&key).map_err(|e| e.to_string())?;
    // Decode the data into a User struct
    let user = result.map(|data| User::decode(&mut &data[..]).unwrap());
    Ok(user)
}
```
