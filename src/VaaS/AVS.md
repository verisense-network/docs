# AVS 

The concept of DApp development can refer to both smart contract development and application chain (a dedicated blockchain system developed for a specific large-scale application) development. Over the past decade, smart contract development has brought significant progress to blockchain applications, but people have also gradually realized the many limitations of the smart contract model: the cost is determined by the host chain, the speed is relatively low, and the programming model is restrictive. The barrier to entry for application chain development is relatively higher, and it requires finding independent verification nodes, making the cost of application chains not low either. Moreover, due to the relatively low degree of decentralization and the dependence on high cross-chain costs, application chains find it difficult to achieve natural compatibility with mainstream public chains.

The term [AVS(Actively Validated Services)](https://docs.eigenlayer.xyz/eigenlayer/avs-guides/avs-developer-guide#what-is-an-avs) is originally proposed by Eigenlayer. It is a new paradigm of DApp development but Eigenlayer doesn't provide such a standard template or framework to develop AVS.

Verisense provides developers with a complete SDK and toolset, making it easier and more efficient to build distributed applications based on AVS. By using Verisense, developers can focus on the core functionality of their applications without having to worry about the underlying verification and monitoring mechanisms.

![](https://raw.githubusercontent.com/verisense-network/verisense-docs/master/assets/avs-overview.png)

The key concepts of Verisense AVS include:

1. Actor Model: In Verisense, the AVS system adopts the common distributed systems concept of the Actor model. However, Verisense uses the term "Nucleus" instead of "Actor" since Verisense is a decentralized system. Each Nucleus corresponds to a DApp, and developers need to use the nucleus-core library and other tools to develop and compile the Nucleus into a WebAssembly (WASM) binary file. As a decentralized Actor model, the Nucleus has the ability to actively send messages to other Nuclei or even external systems. This allows the Verisense AVS to break free from the limitations of blockchain systems, which are often passive in nature.

2. Verisense as a Blockchain: Verisense itself is a blockchain network. In addition to executing the consensus of the Verisense network, each Verisense node can also register as a member of a specific AVS and download the corresponding Nucleus WASM binary code to execute. In other words, Verisense's node operators don't have to run an extra independent AVS node.

3. FHE-enhanced Lightweight Consensus Protocol: Verisense has developed a lightweight consensus protocol called Monadring specifically for the AVS system. In this consensus protocol, data does not reach consensus in blocks, allowing the AVS to respond to data write requests more quickly. In addition to the security benefits of restaking, Verisense has also incorporated Fully Homomorphic Encryption (FHE) into its Monadring consensus protocol. The unique properties of FHE help to eliminate the potential for gaming in the voting process, even in small-scale decentralized systems, allowing Verisense to run small-scale decentralized applications with the same level of theoretical security.
