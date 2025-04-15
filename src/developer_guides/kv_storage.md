## Key Value Storage

Verisense has a full set of API on KV storage. Let's look at it.


### APIs


#### put

Put a value into database via a key.

```
pub fn put(key: impl AsRef<[u8]>, value: impl AsRef<[u8]>) -> CallResult<()> 
```

Example:

```
use vrs_core_sdk::{get, post, storage};

#[post]
pub fn add_user(mut u: User) -> Result<(), String> {
    let key = b"user:001";
    let val: Vec<u8> = u.encode();     
    storage::put(&key, &val).map_err(|e| e.to_string())?;

    Ok(())
}
```

**Note**: `storage::put()` can only be used in the function decorated by `#[post]`.

#### del

Delete a value from database via a key.

```
pub fn del(key: impl AsRef<[u8]>) -> CallResult<()>
```

Example:

```
use vrs_core_sdk::{get, post, storage};

#[post]
pub fn delete_user() -> Result<(), String> {
    let key = b"user:001";

    storage::del(&key).map_err(|e| e.to_string())?;

    Ok(())
}
```

**Note**: `storage::del()` can only be used in the function decorated by `#[post]`.


#### get

Get a value from database via a key.

```
pub fn get(key: impl AsRef<[u8]>) -> CallResult<Option<Vec<u8>>> {
```

Example:

```
#[get]
pub fn get_user() -> Result<Option<User>, String> {
    let key = b"user:001";
    let r = storage::get(&key).map_err(|e| e.to_string())?;
    let instance = r.map(|d| User::decode(&mut &d[..]).unwrap());
    Ok(instance)
}
```

#### get_range

Get a batch of entries from the database with "start_key" and direction, the limit maximum is 1000

```
pub fn get_range(
    start_key: impl AsRef<[u8]>,
    direction: Direction,
    limit: usize,
) -> CallResult<Vec<(Vec<u8>, Vec<u8>)>> 
```

Example:

```
#[get]
pub fn get_user_range() -> Result<(), String>  {
    let prefix_key = b"user:";
    let r = storage::get_range(&key, Direction::Forward, 100).map_err(|e| e.to_string())?;
    ...
}
```


#### delete_range

Removes the database entries in the range [start_key, end_key)

```
pub fn delete_range(start_key: impl AsRef<[u8]>, end_key: impl AsRef<[u8]>) -> CallResult<()> 
```

Example:

```
#[post]
pub fn delete_user_range() -> Result<(), String>  {
    let start_key = b"user:001";
    let end_key = b"user:100";
    let r = storage::delete_range(&start_key, &end_key).map_err(|e| e.to_string())?;
    ...
}
```

**Note**: `storage::delete_range()` can only be used in the function decorated by `#[post]`.
 
#### search

Search a value with a key prefix and direction.

```
pub fn search(
    key_prefix: impl AsRef<[u8]>,
    direction: Direction,
) -> CallResult<Option<(Vec<u8>, Vec<u8>)>> 
 ```

Example:

```
use vrs_core_sdk::storage::Direction;

pub fn search_blog_id() {
   let key = [&b"blog:"[..], &0u64.to_be_bytes()[..]].concat();
   let first_blog = storage::search(&key, Direction::Forward).unwrap();
   let key = [&b"blog:"[..], &u64::MAX.to_be_bytes()[..]].concat();
   let last_blog = storage::search(&key, Direction::Reverse).unwrap();
   assert!(first_blog.is_some());
   assert!(last_blog.is_some());
}
```
