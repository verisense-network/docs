# Interacting with Nucleus

In the [previous chapter](./deploy.md), we deployed a Nucleus with two interfaces:

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

This chapter explains how to interact with this Nucleus.

## Encoding and Decoding

We use Polkadot's [Codec](https://github.com/paritytech/parity-scale-codec) crate to encode and decode data within the Nucleus.

For different programming languages, the following libraries can be used for encoding:

* **AssemblyScript** - [LimeChain/as-scale-codec](https://github.com/LimeChain/as-scale-codec)
* **C** - [MatthewDarnell/cScale](https://github.com/MatthewDarnell/cScale)
* **C++** - [qdrvm/scale-codec-cpp](https://github.com/qdrvm/scale-codec-cpp)
* **JavaScript** - [polkadot-js/api](https://github.com/polkadot-js/api)
* **Dart** - [leonardocustodio/polkadart](https://github.com/leonardocustodio/polkadart)
* **Haskell** - [airalab/hs-web3](https://github.com/airalab/hs-web3/tree/master/packages/scale)
* **Golang** - [itering/scale.go](https://github.com/itering/scale.go)
* **Java** - [splix/polkaj](https://github.com/splix/polkaj)
* **Python** - [polkascan/py-scale-codec](https://github.com/polkascan/py-scale-codec)
* **Ruby** - [wuminzhe/scale\_rb](https://github.com/wuminzhe/scale_rb)
* **TypeScript** - [parity-scale-codec-ts](https://github.com/tjjfvi/subshape), [scale-ts](https://github.com/unstoppablejs/unstoppablejs/tree/main/packages/scale-ts#scale-ts), [soramitsu/scale-codec-js-library](https://github.com/soramitsu/scale-codec-js-library), [subsquid/scale-codec](https://github.com/subsquid/squid-sdk/tree/master/substrate/scale-codec)

For instance, consider the following structure:

```rust
pub struct User {
    pub id: u64,
    pub name: String,
}
```

With `id = 123456` and `name = "nucleus_1"`, the encoded data is:

```
40e2010000000000246e75636c6575735f31
```

To invoke the `add_user` interface via JSON-RPC:

```bash
curl --location 'https://rpc.beta.verisense.network' \
--header 'Content-Type: application/json' \
--data '
{
    "jsonrpc": "2.0",
    "method": "nucleus_post",
    "params": [
        "kGgGtCimpkywYrQ7yULt3pEZYwetW35NrupEfSyTavTPULXbV", // Nucleus ID
        "add_user", // Function name
        "40e2010000000000246e75636c6575735f31" // Encoded parameters
    ],
    "id": 1
}
```

Example response:

```json
{"jsonrpc":"2.0","result":"0040e2010000000000","id":1}
```

To invoke the `get_user` interface via JSON-RPC:

```bash
curl --location 'https://rpc.beta.verisense.network' \
--header 'Content-Type: application/json' \
--data '
{
    "jsonrpc": "2.0",
    "method": "nucleus_get",
    "params": ["kGgGtCimpkywYrQ7yULt3pEZYwetW35NrupEfSyTavTPULXbV", "get_user", "40e2010000000000"],
    "id": 1
}'
```

Example response:

```json
{"jsonrpc":"2.0","result":"000140e2010000000000246e75636c6575735f31","id":1}
```

Decoding the result yields:

```rust
Ok(Some(User { id: 123456, name: "nucleus_1" }))
```
