## Demo: A Decentralized Forum

In this tutorial, I will guide you through the process of building a decentralized forum on Verisense.

To develop a decentralized application (dApp) on Verisense, you'll need to implement four main components:

1. **AVS**: This is a compiled WASM file created using the `vrs-core-sdk`. The front-end interacts with this component to write data.
   
2. **Surrogate**: This is a proxy program responsible for syncing the latest data from the AVS and pushing it to MeiliSearch for efficient searching.

3. **MeiliSearch**: A fast and powerful search engine that will handle data queries from the front-end, enabling a seamless search experience.

4. **Front-end app**: This is the user-facing interface that allows interaction with the forum.

The **AVS** will be deployed on Verisense, while the **Surrogate** and **MeiliSearch** instances will be deployed on the same server node where Verisense is running.

Before we begin, please ensure that you have all the required tools installed.

### AVS


#### Create an empty project

First, create a new Rust project,

```
cargo new --lib veavs
```

Put these into the `Cargo.toml`.

```
[package]
name = "veavs"
version = "0.1.0"
edition = "2021"

[lib]
crate-type = ["cdylib"]

[dependencies]
vrs-core-sdk = { git = "https://github.com/verisense-network/verisense.git", package = "vrs-core-sdk" }
parity-scale-codec = { version = "3.6", features = ["derive"] }

vemodel = { path = "../vemodel" }
```

You can refer to the original file content [here](https://github.com/verisense-network/veforum/blob/main/veavs/Cargo.toml).

#### Define models

For a decentralized forum, we need to define the following models:

```
use parity_scale_codec::{Decode, Encode};
use serde::{Deserialize, Serialize};

#[derive(Debug, Decode, Encode, Deserialize, Serialize)]
pub enum Method {
    Create,
    Update,
    Delete,
}

#[derive(Debug, Decode, Encode, Deserialize, Serialize)]
pub struct VeSubspace {
    pub id: u64,
    pub title: String,
    pub slug: String,
    pub description: String,
    pub banner: String,
    pub status: i16,
    pub weight: i16,
    pub created_time: i64,
}

#[derive(Debug, Decode, Encode, Deserialize, Serialize)]
pub struct VeArticle {
    pub id: u64,
    pub title: String,
    pub content: String,
    pub author_id: u64,
    pub author_nickname: String,
    pub subspace_id: u64,
    pub ext_link: String,
    pub status: i16,
    pub weight: i16,
    pub created_time: i64,
    pub updated_time: i64,
}

#[derive(Debug, Decode, Encode, Deserialize, Serialize)]
pub struct VeComment {
    pub id: u64,
    pub content: String,
    pub author_id: u64,
    pub author_nickname: String,
    pub post_id: u64,
    pub status: i16,
    pub weight: i16,
    pub created_time: i64,
}
``` 

You can refer to the original file content [here](https://github.com/verisense-network/veforum/blob/main/vemodel/src/lib.rs).

#### Implement business

We will implement CRUD actions on each model.

```
// subspace
#[post]
pub fn add_subspace(mut sb: VeSubspace) -> Result<(), String> {
    let max_id = get_max_id(PREFIX_SUBSPACE_KEY);
    // update the id field from the avs
    sb.id = max_id;
    let key = build_key(PREFIX_SUBSPACE_KEY, max_id);
    storage::put(&key, sb.encode()).map_err(|e| e.to_string())?;

    add_to_common_key(Method::Create, key)?;

    Ok(())
}

#[post]
pub fn update_subspace(sb: VeSubspace) -> Result<(), String> {
    let id = sb.id;
    let key = build_key(PREFIX_SUBSPACE_KEY, id);
    storage::put(&key, sb.encode()).map_err(|e| e.to_string())?;

    add_to_common_key(Method::Update, key)?;

    Ok(())
}

#[post]
pub fn delete_subspace(id: u64) -> Result<(), String> {
    let key = build_key(PREFIX_SUBSPACE_KEY, id);
    storage::del(&key).map_err(|e| e.to_string())?;

    add_to_common_key(Method::Delete, key)?;

    Ok(())
}

#[get]
pub fn get_subspace(id: u64) -> Result<Option<VeSubspace>, String> {
    let key = build_key(PREFIX_SUBSPACE_KEY, id);
    let r = storage::get(&key).map_err(|e| e.to_string())?;
    let instance = r.map(|d| VeSubspace::decode(&mut &d[..]).unwrap());
    Ok(instance)
}

// article
#[post]
pub fn add_article(mut sb: VeArticle) -> Result<(), String> {
    let max_id = get_max_id(PREFIX_ARTICLE_KEY);
    // update the id field from the avs
    sb.id = max_id;
    let key = build_key(PREFIX_ARTICLE_KEY, max_id);
    storage::put(&key, sb.encode()).map_err(|e| e.to_string())?;
    add_to_common_key(Method::Create, key)?;

    Ok(())
}

#[post]
pub fn update_article(sb: VeArticle) -> Result<(), String> {
    let id = sb.id;
    let key = build_key(PREFIX_ARTICLE_KEY, id);
    storage::put(&key, sb.encode()).map_err(|e| e.to_string())?;
    add_to_common_key(Method::Update, key)?;

    Ok(())
}

#[post]
pub fn delete_article(id: u64) -> Result<(), String> {
    let key = build_key(PREFIX_ARTICLE_KEY, id);
    storage::del(&key).map_err(|e| e.to_string())?;
    add_to_common_key(Method::Delete, key)?;

    Ok(())
}

#[get]
pub fn get_article(id: u64) -> Result<Option<VeArticle>, String> {
    let key = build_key(PREFIX_ARTICLE_KEY, id);
    let r = storage::get(&key).map_err(|e| e.to_string())?;
    let instance = r.map(|d| VeArticle::decode(&mut &d[..]).unwrap());
    Ok(instance)
}

// comment
#[post]
pub fn add_comment(mut sb: VeComment) -> Result<(), String> {
    let max_id = get_max_id(PREFIX_COMMENT_KEY);
    // update the id field from the avs
    sb.id = max_id;
    let key = build_key(PREFIX_COMMENT_KEY, max_id);
    storage::put(&key, sb.encode()).map_err(|e| e.to_string())?;
    add_to_common_key(Method::Create, key)?;

    Ok(())
}

#[post]
pub fn update_comment(sb: VeComment) -> Result<(), String> {
    let id = sb.id;
    let key = build_key(PREFIX_COMMENT_KEY, id);
    storage::put(&key, sb.encode()).map_err(|e| e.to_string())?;
    add_to_common_key(Method::Update, key)?;

    Ok(())
}

#[post]
pub fn delete_comment(id: u64) -> Result<(), String> {
    let key = build_key(PREFIX_COMMENT_KEY, id);
    storage::del(&key).map_err(|e| e.to_string())?;
    add_to_common_key(Method::Delete, key)?;

    Ok(())
}

#[get]
pub fn get_comment(id: u64) -> Result<Option<VeComment>, String> {
    let key = build_key(PREFIX_COMMENT_KEY, id);
    let r = storage::get(&key).map_err(|e| e.to_string())?;
    let instance = r.map(|d| VeComment::decode(&mut &d[..]).unwrap());
    Ok(instance)
}
```

You can find the full code [here](https://github.com/verisense-network/veforum/blob/main/veavs).

#### Compile to wasm

In the root of this project, run:

```
cargo build --release --target wasm32-unknown-unknown
```

You can find the compiled wasm file located at `target/wasm32-unknown-unknown/release/veavs.wasm`.


#### Deploy it to the Verisense

Register a new AVS protocol on Verisense.

```
vrx nucleus create veavs --capacity 1
```

This command will return the registered AVS (nucleus) ID like:

```
Nucleus created.
  id: 5FsXfPrUDqq6abYccExCTUxyzjYaaYTr5utLx2wwdBv1m8R8
  name: hello_avs
  capacity: 1
```

Deply the generated wasm file to Verisense using the generated Nucleus ID.

```
vrx deploy --name veavs --wasm-path ../target/wasm32-unknown-unknown/release/veavs.wasm --nucleus-id 5FsXfPrUDqq6abYccExCTUxyzjYaaYTr5utLx2wwdBv1m8R8  --version 1
```

Wait for the process to complete successfully.

At this point, we have successfully deployed a new **AVS** onto Verisense.

### Surrogate

The AVS functions as a raw database, but to make use of this data, we need to create a proxy that will index the data into the **MeiliSearch** engine.

You can check out the surrogate implementation [here](https://github.com/verisense-network/veforum/blob/main/surrogate/src/main.rs).

The basic concept behind the surrogate is to retrieve data from the AVS and inject it into MeiliSearch for efficient indexing and searching.

### MeiliSearch

MeiliSearch provides a standardized approach for handling data queries.

You can find the API documentation [here](https://www.meilisearch.com/docs/reference/api/search).

### Front-end

You can find the reference code for the front-end [here](https://github.com/verisense-network/veforum-frontend).

In the front-end application, the logic involves writing data to the **AVS** and querying data from **MeiliSearch** to display the results.

### What It Looks Like

For a preview of the app in action, check out this [video](https://www.youtube.com/watch?v=Clb5NqqYUPk).
