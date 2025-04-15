# Monadring

The Monadring protocol is an essential subprotocol within the Verisense ecosystem, designed to attain consensus for Nucleus operations. It is engineered to function effectively even in small-scale decentralized networks by leveraging an underlying blockchain network, specifically the Verisense Hostnet. Our rigorous design and analysis of the Monadring protocol are documented in a paper available on [arXiv](https://arxiv.org/abs/2408.16094). This section offers a concise overview of its foundational principles.

## Subnet Topology
Monadring defines a topological structure among network members, forming a ring where all members are connected end-to-end. This ring structure is established by the sorting of Verisense validators via submitted Verifiable Random Function (VRF) proofs as part of their candidacy.

## Token Circulation
Within a subnet, a token circulates periodically around the ring, granting state modification rights to the node currently holding it. Though the token structure is complex and detailed in our paper, it can be simplistically understood as containing each node's received events, the current state, and node signatures. When a node receives the token, it first executes events enclosed within, originating from other nodes, followed by its own events. Upon execution, it adds its events to the token, propagating it around the network. This ensures a globally recognized sequence of Nucleus events, defining the sequence of state modifications.

![](https://raw.githubusercontent.com/verisense-network/docs/refs/heads/master/assets/ring.png)

## Full Homomorphic Encryption (FHE)
A network relying solely on VRF for random member selection lacks inherent security. Thus, we introduce FHE to sign the states contained within the token, ensuring that nodes cannot view the processing results of others for specific events. By utilizing VRF for node selection and FHE for token signing, the subnet consensus mechanism simulates a Prisoner's Dilemma scenario. Even in small-scale networks, appropriately designed incentive strategies can enforce network security.

## Consensus on Network Requests
Nucleus state changes are abstractly referred to as events, akin to ledger-modifying transactions within traditional blockchains. Verisense enhances this with network request capabilities, necessitating special consensus treatment for such requests.

### Handling Network Requests
Network requests within a Nucleus are partitioned into two events: request initiation and response reception. Developers initiate a network request by calling an asynchronous function, returning a request_id. For the execution environment, nodes dispatch the request while recording TLS handshake keys as parameters. Nodes execute events only when holding the token, ensuring a fair distribution of request execution across the network.

The consensus for request events is straightforward since given the same event sequence, all nodes generate a network request event. However, network request events within the token are not executed again, as their presence indicates prior execution by a token-holding node.

### Handling Network Response Events
HTTPS server certificates and shared handshake keys from the request-initiating node allow deterministic session key computation through the Diffie-Hellman handshake process. Only the node that issued the original request will receive a response. This HTTP response, encrypted with the session key, is set as a response event in the token and passed to other nodes. Upon receiving this event, nodes decrypt and execute it independently.

## Consensus on Timers
Timers present similar challenges due to their reliance on local system time and scheduling, which cannot be synchronized perfectly across all nodes. Thus, timers are divided into two events: timer setup and timer trigger. Within a subnet, only one node will actually trigger a scheduled timer; the rest will recognize the trigger through token transmission and disable their local timers upon receiving the event.

## Conclusion
The Monadring protocol enables Nucleus consensus, balancing flexibility and security in decentralized applications within Verisense. By organizing a strategic combination of VRF, FHE, and unique event handling mechanisms, Monadring supports secure and efficient consensus even in small networks. Detailed exploration of this protocol's intricacies can be found in our arXiv publication, complementing this overview with a deeper theoretical and technical foundation.
