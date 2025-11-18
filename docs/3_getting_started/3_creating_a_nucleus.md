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
vrs-core-sdk = { version = "0.2.0" }
parity-scale-codec = { version = "3.6", features = ["derive"] }
scale-info = { version = "2.11.6", features = ["derive"] }
```

### Step 3: Implement the Core Logic

Edit the file `src/lib.rs` and insert the following code:

```rust
use scale_info::TypeInfo;
use vrs_core_sdk::codec::{Decode, Encode};
use vrs_core_sdk::nucleus;
#[derive(Debug, Decode, Encode, TypeInfo)]
pub struct User {
    pub id: u64,
    pub name: String,
}
#[nucleus]
pub mod nucleus {
    use crate::User;
    use vrs_core_sdk::codec::{Decode, Encode};
    use vrs_core_sdk::{get, post, storage};
    #[post]
    pub fn add_user(user: User) -> Result<u64, String> {
        let key = [&b"user:"[..], &user.id.to_be_bytes()[..]].concat();
        println!("{:?}", key);
        storage::put(&key, &user.encode()).map_err(|e| e.to_string())?;
        Ok(user.id)
    }

    #[get]
    pub fn get_user(id: u64) -> Result<Option<User>, String> {
        let key = [&b"user:"[..], &id.to_be_bytes()[..]].concat();
        println!("{:?}", key);
        let result = storage::get(&key).map_err(|e| e.to_string())?;
        let user = result.map(|data| User::decode(&mut &data[..]).unwrap());
        Ok(user)
    }
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
#[derive(Debug, Decode, Encode, TypeInfo)]
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
  
### Purpose of `TypeInfo` and `#[nucleus]`

#### `TypeInfo`

```rust
#[derive(Debug, Decode, Encode, TypeInfo)]
pub struct User {
    pub id: u64,
    pub name: String,
}
```

The `TypeInfo` derive macro comes from the [`scale-info`](https://docs.rs/scale-info) crate. It plays a crucial role in generating **type metadata at compile time**, which is then embedded into your compiled WebAssembly module.

This metadata serves several important purposes:

* **ABI Generation:**
  `TypeInfo` enables automatic generation of an Application Binary Interface (ABI). This allows external clients (such as frontends, other chain modules, or the Verisense dashboard) to understand the precise structure of types like `User`, including their field names and data types.

* **Cross-language Compatibility:**
  Since the type descriptions are included, tools written in JavaScript, Python, or other languages can introspect your WebAssembly module and correctly encode/decode the data to call your functions.

* **Auto-generated UI:**
  The Verisense dashboard, for example, uses this type metadata to automatically generate input forms. It knows that when it needs a `User`, it should display an input for `id: u64` and `name: String`.

In short:

> **`TypeInfo` ensures that your data structures are fully described for ABI export, enabling automated tooling, documentation, and seamless cross-language integration.**

---

#### `#[nucleus]`

```rust
#[nucleus]
pub mod nucleus {
    ...
}
```

The `#[nucleus]` attribute macro comes from the `vrs-core-sdk` and is fundamental to marking this Rust module as a **Nucleus**.

Its primary responsibilities are:

* **Export ABI:**
  It collects all functions inside this module that are marked with `#[get]`, `#[post]`, or `#[init]` and registers them in the Nucleus ABI. This means when your `.wasm` is deployed, the blockchain runtime or the Verisense dashboard knows exactly which functions are exposed, along with their input/output types.

* **Generate Glue Code:**
  It automatically generates the necessary export functions (such as `_invoke`) that the blockchain’s Wasm executor will call. This abstracts away low-level host bindings, so you only need to focus on your Rust functions.

* **Provide a Clean Namespace:**
  By wrapping your interfaces inside the `#[nucleus]` macro, you avoid polluting the global scope. All public functions intended to be callable externally are neatly contained and registered.

In short:

> **`#[nucleus]` transforms your Rust module into a deployable AVS, automatically exporting its ABI and wiring up the execution glue so it can run inside the Verisense runtime.**


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
