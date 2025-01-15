# Quick Start Guide: Developing Nucleus on Verisense

Welcome to Verisense! This guide will help you quickly get started with developing your first Nucleus using Rust. By the end, you’ll have a simple deployed Nucleus and be able to interact with it.

## 1. Set Up Your Rust Environment

First, install Rust and configure it for WebAssembly (Wasm) compilation:

### Install Rust:

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```
### Add the WebAssembly target:
```bash
rustup target add wasm32-unknown-unknown
```

## 2. Create and Compile a Rust Project

### Create a new Rust library:

```bash
cargo new --lib hello-avs
cd hello-avs
```

### Update Cargo.toml

```toml
[package]
name = "hello-avs"
version = "0.1.0"
edition = "2021"

[lib]
crate-type = ["cdylib"]

[dependencies]
vrs-core-sdk = { version = "0.0.2" }
parity-scale-codec = { version = "3.6", features = ["derive"] }
```

### Write your first Nucleus code:
```rust
use parity_scale_codec::{Decode, Encode};
use vrs_core_sdk::{get, post, storage};

#[derive(Debug, Decode, Encode)]
pub struct User {
    pub id: u64,
    pub name: String,
}

#[post]
pub fn add_user(user: User) -> Result<u64, String> {
    let max_id_key = [&b"user:"[..], &u64::MAX.to_be_bytes()[..]].concat();
    let max_id = match storage::search(&max_id_key, storage::Direction::Reverse)
        .map_err(|e| e.to_string())?
    {
        Some((id, _)) => u64::from_be_bytes(id[5..].try_into().unwrap()) + 1,
        None => 1u64,
    };
    let key = [&b"user:"[..], &max_id.to_be_bytes()[..]].concat();
    storage::put(&key, user.encode()).map_err(|e| e.to_string())?;
    Ok(max_id)
}

#[get]
pub fn get_user(id: u64) -> Result<Option<User>, String> {
    let key = [&b"user:"[..], &id.to_be_bytes()[..]].concat();
    let r = storage::get(&key).map_err(|e| e.to_string())?;
    let user = r.map(|d| User::decode(&mut &d[..]).unwrap());
    Ok(user)
}
```

### Build the project for WebAssembly:

```bash
cargo build --release --target wasm32-unknown-unknown
```

## 3. Install Command-Line Tools and Get Free Gas

### Install the Verisense CLI:
```bash
cargo install --git https://github.com/verisense-network/vrs-cli.git
```

### Generate an account:
```bash
vrx account generate --save
```

This command will generate an account and save the private key to ~/.vrx/default-key. Example output:
```
Phrase: exercise pipe nerve daring census inflict cousin exhaust valve legend ancient gather
Seed: 0x35929b4e23d26c5ba94d22d32222128e56f5a7dce35f9b36b467ac2be2b4d29b
Public key: 0x9cdaa67b771a2ae3b5e93b3a5463fc00e6811ed4f2bd31a745aa32f29541150d
Account Id: kGj5epfCkuae7DJpezu5Qx6mp96gHmLv2kDPHHTdJaEVNptRt
```

### Request free gas:

Chat with the [Verisense Faucet Bot](https://t.me/verisense_faucet_bot) and provide your account ID to request free $VRS.

## 4. Create and Deploy a Nucleus

### Create a Nucleus:
```bash
vrx nucleus --devnet create --name hello_avs --capacity 1
```

Example output:

```
Nucleus created.
  id: kGieDqL1fX8J7n1vRbXri7DVphwnZJpkDcoMoQZWo9XkTt1Sv
  name: hello_avs
  capacity: 1
```

### Deploy the compiled Wasm:
```
vrx install --wasm target/wasm32-unknown-unknown/release/hello_avs.wasm --id kGieDqL1fX8J7n1vRbXri7DVphwnZJpkDcoMoQZWo9XkTt1Sv
```

If successful, you will see output like this:

```
Digest: 0xff878e546806da8b13f02765ea84f616963abcfdcac196ba3ea9f3f5d94b661e
Peer ID: 12D3KooWCz46orfkSfaahJqkph1bQqXU9t7ct98YQKTaDepNE6du
Transaction submitted: "0x0bc7d23b900a880e5274582755fc1a6c17df9453b0aa43f4cc382efc0bf1ec39"
```

## 5. Test Your Nucleus

### Call add_user:

```bash
curl https://alpha-devnet.verisense.network -H 'Content-Type: application/json' -XPOST -d '{"jsonrpc":"2.0", "id":"whatever", "method":"nucleus_post", "params": ["kGieDqL1fX8J7n1vRbXri7DVphwnZJpkDcoMoQZWo9XkTt1Sv", "add_user", "000000000000000014416c696365"]}'
```

The networking component follows the standard JSON-RPC specification, and all post and get methods share a same endpoint separately. In the *add_user* case, the method is *nucleus_post* and so all other post methods.

The first parameter is the *nucleus_id* we just deployed, the second indicates the function name in the source code which is *add_user*. While the third is an *parity-scale-encoded* bytes whose value is: `User {0, "Alice"}`. You can find different implementations for various programming languages.


### Call get_user:

Calling get_user is similar, we just need to change the method and parameter:

```bash
curl https://alpha-devnet.verisense.network -H 'Content-Type: application/json' -XPOST -d '{"jsonrpc":"2.0", "id":"whatever", "method":"nucleus_get", "params": ["kGieDqL1fX8J7n1vRbXri7DVphwnZJpkDcoMoQZWo9XkTt1Sv", "get_user", "0100000000000000"]}'
```

## Conclusion

Congratulations! You’ve created, deployed, and interacted with your first Nucleus on Verisense. You can now expand your AVS functionality and explore advanced features of the platform.

For more information, check the Verisense documentation.

## What's next
For more advanced topics, see:
- [Making http requests]()
- [Setting a timer]()
- [Deriving external addresses to hold external assets]()
- [Signing an external signature]()
- [Demo: developing a decentralized forum]()
