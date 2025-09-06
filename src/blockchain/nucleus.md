# Nucleus

In Verisense, a Nucleus represents a decentralized application running within a subnet. This section delves into the capabilities of a Nucleus, which is compiled into WebAssembly (WASM) bytecode, allowing for efficient execution within the Verisense framework. As previously mentioned in the Introduction, decentralized applications should operate within a cost-effective decentralized environment. In Verisense, the degree of decentralization, determined by the number of nodes securing a Nucleus, is customizable by developers to align with the application’s security needs. The process of achieving consensus among multiple nodes is discussed in detail in the "Monadring" section. Here, we will explore the specific capabilities that Verisense provides for Nucleus applications.

## Reverse Gas Mode

Traditional blockchain systems typically employ a "pay-to-write" model, where the actor modifying the ledger incurs a cost (e.g., deploying contracts, changing contract states). This model has long posed a barrier for broad user adoption beyond the realm of Web3 enthusiasts. Verisense innovates with a reverse gas mode, where the platform charges the publisher of the Nucleus for usage. This pricing model resembles that of cloud service providers like AWS. By default, users can interact with a Nucleus (both reads and writes) free of charge, unless the developer explicitly chooses otherwise. This setup aligns more closely with traditional web applications, where certain API calls may require user authentication or payment, while others remain freely accessible.

## Feature-Rich SDK

Most blockchain systems primarily offer two functionalities: key-value database read/write operations and signature verification. While smart contract virtual machines introduce Turing-complete development capabilities, the user experience often falls short compared to equivalent Web2 applications. Verisense aims to bridge this gap by offering a robust SDK for Nucleus development, featuring capabilities rarely found in other blockchains:

- **Proactive Network Requests**: Nucleus can autonomously initiate network requests, enabling dynamic interactions with external data sources and systems.

- **Timers**: Developers can set timers within a Nucleus to trigger events or operations at scheduled intervals, enhancing application functionality and automation.

- **Multitype Public Key Access and Signature Functions**: Nucleus can obtain various public key types and execute functions to sign any data.

The picture below shows some use cases of Nucleus.

![](https://raw.githubusercontent.com/verisense-network/docs/refs/heads/master/assets/usage.png)

## Lifecycle

The lifecycle of a Nucleus in Verisense encompasses several distinct stages, from creation through operation and potential decommissioning.

1. **Creation**
The creation of a Nucleus is initiated through a legitimate transaction on the Verisense Hostnet. Developers can utilize the `vrx` command-line tool to facilitate this process. To install the `vrx` tool, use the following command:

```
cargo install --git https://github.com/verisense-network/vrs-cli.git
```

Note: Verisense is subject to rapid development, hence frequent updates may be required for `vrx`. Please refer to the Developer Guides for detailed instructions.

2. **WASM Update**

In Verisense, the code of a Nucleus is an integral part of its state. This unification implies that there is no distinction between the initial deployment of code and subsequent updates. The initial deployment of a Nucleus’s WebAssembly (WASM) code is logged as the zeroth event in the Nucleus’s lifecycle.

3. **Recovery**

Subnet member nodes assigned to a Nucleus initiate an additional WASM virtual machine (different from the Verisense Hostnet) dedicated to operating the Nucleus. These nodes expose the Nucleus's interfaces via an RPC endpoint. Verisense implements a sophisticated billing model that tracks charges based on the following activities:

- Storage usage
- Data write requests
- Invocation of system functions

Each time the state root of a Nucleus is synchronized with the Hostnet, the corresponding account address of the Nucleus is automatically debited with the accrued costs.

Should the balance of a Nucleus's account fall below a predetermined threshold, Verisense will cease to process requests associated with the Nucleus until additional funds are deposited. This mechanism ensures that network resources are allocated efficiently and that the operation of Nucleuses remains financially sustainable.

## Indexer

In blockchain networks, maintaining consensus requires that state updates are processed with deterministic time complexity. Consequently, blockchain storage is typically restricted to key-value (KV) databases, where queries and modifications have predictable time complexity. Verisense follows this same principle, with each Nucleus possessing its own isolated storage space implemented using RocksDB.

However, to enable advanced querying capabilities, an additional component akin to a blockchain explorer is often necessary. In the context of a Nucleus, such a component is referred to as an "Indexer," designed to facilitate complex business information queries. Unlike blockchain explorers, which serve the entire network, the Indexer for a Nucleus is a specialized off-chain component tailored by the developer for specific use cases within their application.

The implementation of an Indexer is at the discretion of the Nucleus developer, allowing for flexibility and adaptability to various business requirements. Developers can leverage a range of technologies to build their Indexers, including:

- Traditional relational databases
- Full-text search engines
- Services like AWS serverless architectures

This flexibility enables developers to optimize data indexing and querying based on the particular needs of their application.

## Online Demo: Aitonomy

This nucleus demonstrates the abilities of Verisense including bridgeless connection with external blockchains using TSS and AI integration using networking requests.

- [Aitonomy: The first AI-driven decentrailized forum for tokenizing communities](https://www.aitonomy.world)

- [Video on X](https://x.com/veri_sense/status/1901844739378606523?s=46)
