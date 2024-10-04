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

Then install the verisense cli tool `vrx`:

```
cargo install --git https://github.com/verisense-network/vrs-cli.git
```

The command below shows how to create an AVS using the test account `Alice` which already hold some tokens:
```
vrx create-nucleus --name hello_avs --capacity 1 --alice
```
The executing result is something like:
```
Nucleus created.
  id: 5FsXfPrUDqq6abYccExCTUxyzjYaaYTr5utLx2wwdBv1m8R8
  name: hello_avs
  capacity: 1
```
The `id` represents the AVS account, then we deploy our wasm blob using the `id`:
```
vrx deploy --name hello_avs --wasm-path ../hello-avs/target/wasm32-unknown-unknown/release/hello_avs.wasm --nucleus-id 5FsXfPrUDqq6abYccExCTUxyzjYaaYTr5utLx2wwdBv1m8R8 --alice
```
If everything works fine, it will return something like:
```
Digest: 0xff878e546806da8b13f02765ea84f616963abcfdcac196ba3ea9f3f5d94b661e
Peer ID: 12D3KooWCz46orfkSfaahJqkph1bQqXU9t7ct98YQKTaDepNE6du
Transaction submitted: "0x0bc7d23b900a880e5274582755fc1a6c17df9453b0aa43f4cc382efc0bf1ec39"
```

Now it's time to request our AVS. Let's call `add_user` first.

```
curl localhost:9944 -H 'Content-Type: application/json' -XPOST -d '{"jsonrpc":"2.0", "id":"whatever", "method":"nucleus_post", "params": ["5FsXfPrUDqq6abYccExCTUxyzjYaaYTr5utLx2wwdBv1m8R8", "add_user", "000000000000000014416c696365"]}'
```
The networking component follows the standard JSON-RPC specification, and all `post` and `get` methods share a same endpoint seperately. In the `add_user` case, the method is `nucleus_post` and so all other `post` methods.

The first parameter is the `nucleus_id` we are requesting, the second indicates the function name in the source code which is `add_user`. While the third is an encoded bytes whose value is: ```User {0, "Alice"}```. You can find different implementations for various programming languages.

Calling `get_user` is similar, we just need to change the method and parameter:
```
curl localhost:9944 -H 'Content-Type: application/json' -XPOST -d '{"jsonrpc":"2.0", "id":"whatever", "method":"nucleus_post", "params": ["5FsXfPrUDqq6abYccExCTUxyzjYaaYTr5utLx2wwdBv1m8R8", "get_user", "0100000000000000"]}'
```


## What's next
For more advanced topics, see:
- [Making http requests]()
- [Setting a timer]()
- [Deriving external addresses to hold external assets]()
- [Signing an external signature]()
- [Demo: developing a decentralized forum]()
