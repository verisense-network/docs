## Making a Request

To make a http request in Verisense nucleus, you have to split the process into two parts:

1. make request: making a http request and return the request_id immediately;
2. get the callback: a `#[callback]` function will be called with the request_id when the response is ready. 

For example, let's request the `https://www.google.com`.

```
use vrs_core_sdk::{CallResult, http::{*, self}, callback, post};

#[post]
pub fn request_google() {
    let id = http::request(HttpRequest {
        head: RequestHead {
            method: HttpMethod::Get,
            uri: "https://www.google.com".to_string(),
            headers: Default::default(),
        },
        body: vec![],
    })
    .unwrap();
    vrs_core_sdk::println!("http request {} enqueued", id);
}

#[callback]
pub fn on_response(id: u64, response: CallResult<HttpResponse>) {
    match response {
        Ok(response) => {
            let body = String::from_utf8_lossy(&response.body);
            vrs_core_sdk::println!("id = {}, response: {}", id, body);
        }
        Err(e) => {
            vrs_core_sdk::eprintln!("id = {}, error: {:?}", id, e);
        }
    }
}
```

You have to maintain the request ids by a global structure such as a Hashmap.

