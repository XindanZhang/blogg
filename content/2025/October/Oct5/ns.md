# Building overlay network with Vxlan

After trying a whole day working on building overlay network with Vxlan with either `iptables` or `[rtnetlink](https://docs.rs/rtnetlink/latest/rtnetlink/)`, i failed to connect 2 physical machines with different private network stack respectively.

The [example](https://github.com/rust-netlink/rtnetlink/blob/main/examples/create_vxlan.rs) for creating a vxlan interface is here:

```rust
use std::env;

use futures::stream::TryStreamExt;
use rtnetlink::{new_connection, Error, Handle, LinkVxlan};

#[tokio::main]
async fn main() -> Result<(), String> {
    let args: Vec<String> = env::args().collect();
    if args.len() != 2 {
        usage();
        return Ok(());
    }
    let link_name = &args[1];

    let (connection, handle, _) = new_connection().unwrap();
    tokio::spawn(connection);

    create_vxlan(handle, link_name.to_string())
        .await
        .map_err(|e| format!("{e}"))
}

async fn create_vxlan(handle: Handle, name: String) -> Result<(), Error> {
    let mut links = handle.link().get().match_name(name.clone()).execute();
    if let Some(link) = links.try_next().await? {
        let message = LinkVxlan::new("vxlan0", 10)
            .dev(link.header.index)
            .up()
            .port(4789)
            .build();

        handle.link().add(message).execute().await?
    } else {
        println!("no link link {name} found");
    }
    Ok(())
}
```
