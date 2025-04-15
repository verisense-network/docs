# Introduction to Verisense Architecture

Verisense represents a distinctive approach to blockchain architecture by implementing a dual-layer network model. This configuration is specifically designed to address the limitations of traditional blockchain systems and to enable more agile and functional application development.

## Hostnet

The first layer of Verisense, known as the Hostnet, is a Proof-of-Stake (PoS) network constructed using the Substrate framework. At first glance, this may seem conventional, as it lacks support for Ethereum Virtual Machine (EVM) contracts; however, this is an intentional design choice. After over a decade of blockchain innovation, Verisense recognizes that the current paradigm of smart contracts has reached an innovation plateau. Consequently, Verisense deviates from the conventional smart contract virtual machine model, directing all application operations to the second layer, the Subnet.

## Subnet

An application within Verisense are referred to as a Nucleus, and each nucleus operates on an independent Subnet. A subnet is essentially a subset of Hostnet members. This architecture allows each Verisense application to determine its unique consensus requirements, selecting only the necessary nodes for verification based on its specific characteristics and needs. This strategy is inspired by the concept of restaking but extends it further by providing a set of primitive-level Software Development Kits (SDKs) for application development.

Each Subnet functions semi-autonomously, allowing developers to tailor the network’s governance and operational model to best fit the application’s needs. This reduces unnecessary overhead and increases the efficiency and scalability of decentralized applications (dApps).

![](https://raw.githubusercontent.com/verisense-network/docs/refs/heads/master/assets/mixnet.png)

## Advantages Over Traditional Smart Contracts

Unlike traditional smart contracts, Nuclei offer enhanced capabilities that empower developers to create more powerful applications in web2 development way. Key features include:

- Active Network Requests: A nucleus can initiate network requests, enabling them to interact with external systems and data sources such as LLMs or other blockchains.

- Subnet-Level Multi-Type Threshold Signatures: This feature allows a Nucleus to hold some different types of private key such as EcDSA over secp256k1, Ed25519 or Schnorr over secp256k1 and sign arbitrary data. That enables the Nucleus naturally integrate with specific blockchains.

- Timers: The tool is especially beneficial for applications requiring routine operations, scheduled data processing, or time-sensitive triggers.

Verisense's architecture is a forward-thinking approach that breaks away from the limitations of conventional blockchain frameworks. By eschewing the traditional smart contract model and introducing a nuanced dual-layer system, Verisense enables developers to build more robust, flexible, and efficient applications. Its innovative use of subnets and the Nucleus application model marks a significant step forward in the evolution of blockchain technology, positioning Verisense as a pivotal player in the advancement of decentralized solutions. Further technical details and implementation guidelines will be elaborated on in subsequent chapters of this documentation.
