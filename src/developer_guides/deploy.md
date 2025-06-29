# How to Deploy a Nucleus

The previous chapter, [Creating a Simple Nucleus](./simple.md), introduced the steps for creating a basic Nucleus. In step 4, we compiled a Rust project into WebAssembly, resulting in a file named `hello_avs.wasm`. This chapter covers the deployment process for this `hello_avs.wasm` file on the Verisense network.

## Preparing Your Environment

First, install the Verisense command-line tools. Execute the following command in any directory:

```bash
cargo install --git https://github.com/verisense-network/vrs-cli.git
```

If `vrx --version` runs successfully, the installation is complete.

## Creating a Verisense Account

Run the following command to generate an account and save the private key:

```bash
vrx account generate --save
```

This command generates an account and stores the private key in `~/.vrx/default-key`. Example output:

```
Phrase: exercise pipe nerve daring census inflict cousin exhaust valve legend ancient gather
Seed: 0x35929b4e23d26c5ba94d22d32222128e56f5a7dce35f9b36b467ac2be2b4d29b
Public key: 0x9cdaa67b771a2ae3b5e93b3a5463fc00e6811ed4f2bd31a745aa32f29541150d
Account Id: kGj5epfCkuae7DJpezu5Qx6mp96gHmLv2kDPHHTdJaEVNptRt
```

### `.vrx` Directory Structure

The `~/.vrx` directory stores multiple keys, typically named after their respective public keys. The `default-key` file points to the current seed in use. You can verify if the seed you're using matches the one stored in `default-key`.

## Obtaining Test Tokens

Interact with the [Verisense Faucet Bot](https://t.me/verisense_faucet_bot) on Telegram. Provide your account ID to request free \$VRS tokens for testing purposes.

## Deploying the Nucleus

**Step 1: Create a Nucleus**

Execute the command:

```bash
vrx nucleus create hello_avs --rpc wss://rpc.beta.verisense.network --capacity 1
```

This will return a unique Nucleus ID within the Verisense network. Example output:

```
Nucleus created.
  ID: kGgGtCimpkywYrQ7yULt3pEZYwetW35NrupEfSyTavTPULXbV
  Name: hello_avs
  Capacity: 1
```

**Step 2: Install the Nucleus Executable**

Deploy the WebAssembly file with the following command:

```bash
vrx nucleus install --id kGgGtCimpkywYrQ7yULt3pEZYwetW35NrupEfSyTavTPULXbV --wasm hello_avs.wasm --rpc wss://rpc.beta.verisense.network
```

Example output:

```
Transaction submitted: "0xa93f13635f1e3e9ee1a774cd920792f414a41c21d02464919db75010b6763ae6"
```

Creating a Nucleus and installing its executable file are two distinct operations. After creating the Nucleus, the Verisense network assigns a unique Nucleus ID, which will be used for subsequent deployments. Although the Nucleus code is upgradeable, the Nucleus ID remains fixed.
