## Starting a Local Verisense Node

First, we need to clone the Verisense node repository:

```bash
git clone https://github.com/verisense-network/verisense
```

Then, navigate into the project directory:

```bash
cd verisense
```

Next, we build the node binary in release mode:

```bash
cargo build --release
```

Once the build is complete, we can start a local Verisense node. Here, we start an Alice development node with RPC enabled on port 9944:

```bash
target/release/verisense --alice --dev --unsafe-rpc-external --rpc-port 9944
```

After running this command, you will see that the local node has started successfully and is running as an **Alice** node.

This local node will allow us to deploy and interact with our Nucleus for development and testing purposes.

### Debugging on Beta Network

To access logs for debugging purposes on the beta network, visit:
https://rpc.beta.verisense.network/<NUCLEUS_ID>/logs