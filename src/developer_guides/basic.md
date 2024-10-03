# Developer Guides

> Please note: the `vrs-core-sdk` has not been released yet, this document is only for developer preview.


## Quick start

Although all programming languages support WASM could be used for developing AVS on Verisense, we only provide the rust sdk at present. 

For pre-requirement, you need to configurate your rust environment:

```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

```

After installed, add the wasm compiler target:
```
rustup target add wasm32-unknown-unknown
```


Let's create a hello-avs project:

```
cargo new --lib hello-avs
```

To develop a hello-world AVS, you need to indicate the target to `cdylib` and add `vrs-core-sdk` and `parity-scale-codec` as dependencies. Modify the `Cargo.toml`:

```
[package]
name = "hello-avs"
version = "0.1.0"
edition = "2021"

[lib]
crate-type = ["cdylib"]

[dependencies]
vrs-core-sdk = { git = "git@github.com:verisense-network/verisense.git", package = "vrs-core-sdk" }
parity-scale-codec = { version = "3.6", features = ["derive"] }
```

Now, let's develop a simple AVS for naively storing users:

```
use parity_scale_codec::{Decode, Encode};
use vrs_core_sdk::{get, post, storage};

#[derive(Debug, Decode, Encode)]
pub struct User {
    pub id: u64,
    pub name: String,
}

#[post]
pub fn add_user(user: User) -> Result<(), String> {
    let max_id_key = [&b"user:"[..], &u64::MAX.to_be_bytes()[..]].concat();
    let max_id = match storage::search(&max_id_key, storage::Direction::Reverse)
        .map_err(|e| e.to_string())?
    {
        Some((id, _)) => u64::from_be_bytes(id[5..].try_into().unwrap()) + 1,
        None => 1u64,
    };
    storage::put(&max_id.to_be_bytes(), user.encode()).map_err(|e| e.to_string())
}

#[get]
pub fn get_user(id: u64) -> Result<Option<User>, String> {
    let r = storage::get(&id.to_be_bytes()).map_err(|e| e.to_string())?;
    let user = r.map(|d| User::decode(&mut &d[..]).unwrap());
    Ok(user)
}
```

In the top of the source code, we involved two crates: 
- The `parity-scale-codec` to serialize and deserialize the parameters and result.
- The `vrs-core-sdk` contains two macros and a sub module storage.

AVS on Verisense has 2 types of endpoint to access:
- `get`: for read only. Modifying data or initiating external HTTP requests will cause panic.
- `post`: allow modifying the AVS state, initiating external HTTP requests and setting timers.

Both require that the parameters and returning type of the function to impl `Encode + Decode`.

The `add_user` uses the `storage::search` and `storage::put` functions provided by the sdk to lookup the maximum user id and then assign a new id to the incoming user. And the `get_user` simply retrieve it.

To compile the AVS, run:
```
cargo build --release --target wasm32-unknown-unknown
```

Now let't deploy our first AVS to the local node. First, install the recent release version of the Verisense node:

```
git clone git@github.com:verisense-network/verisense.git
cd verisense && cargo build --release
```

After a while, run the Verisense node in dev mode in the name of well-known validator alice:

```
target/release/verisense --dev --alice
```

Then download the verisense cli tool `vrx`:

```
cargo install --git https://github.com/verisense-network/vrs-cli.git
vrx deploy hello-avs/target/wasm32-unknown-unknown/release/hello_avs.wasm
```

## What's next
For more advanced topics, see:
- [Making http requests]()
- [Setting a timer]()
- [Deriving external addresses to hold external assets]()
- [Signing an external signature]()
- [Demo: developing a decentralized forum]()
