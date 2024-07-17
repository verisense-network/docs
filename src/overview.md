# Overview of Verisense

While leading restaking infrastructures like EigenLayer, Karak, and Symbiotics compete fiercely on the TVLs, the supply side of security, less attention has been given to the AVS (Active Validation Services) solution, the demand side of security. It is the demand that drives supply, not the other way around. EigenLayer recognizes this critical aspect but struggles to address it due to the absence of essential components in its underlying design, which are necessary to form a rapid and secure consensus and a fair and functional slashing mechanism.
Verisense Network seizes this market opportunity by proposing an innovative mechanism to fill the gap, addressing the AVS market's needs effectively.

## What is Verisense Network?

Verisense is the world’s first FHE-enabled (Fully Homomorphic Encryption) VaaS (Validation-as-a-Service) network designed to plug and play with any restaking layers. Our goal is to serve AVS in variety (chain-natured, non-chain-natured, and hybrid) and onboard diversified paying AVS clients, a sector currently underserved yet ultimately critical to win the restaking. Here’s what this entails:

### Serving Diverse AVS Clients:

- AVS clients varies in many formats, broadly categorized into i) chain-natured (i.e. sequencers, side chains, oracles),  ii) non-chain-natured (i.e. keeper networks, trust execution environment, threshold cryptography schemes, new virtual machine, decentralized web2 social apps and etc)  and iii) hybrid (i.e. bridge can be implemented as a chain or non-chain format)
- Chain-based AVS clients are easier to onboard but less motivated to pay for decentralized security.
- The true paying demand lies with non-chain-based AVS clients as they don’t have an existing ready-to-use AVS-based infrastructure solution, a sector that leading restaking infrastructures all struggle to serve now.

### Standardizing AVS Demand:

- The lack of standardization and productization in the AVS demand side hinders the growth of an AVS marketplace.
- Verisense aims to offer a standard onboarding process, an easy plug-and-play interface, flexible and reasonable pricing, and value-added functions such as on-duty-operator dashboards and cluster analytics tools.

### Innovating the Consensus Mechanism:

- Forming consensus inexpensively and securely is a challenge. Faster consensus often means less security, and vice versa.
- Verisense proposes an innovative mechanism that decouples runtimes and consensus, crucial for serving non-chain-based AVS clients, for the purpose of implementing the reward/slashing mechanism. 

### Elevating Security and Resilience with FHE Enablement:

- FHE technology allows computations and operations on encrypted data as if it were plain data, thus elevating the security to a new height in the blockchain in general, by enabling flash auctions of block produce and facilitating privacy transactions. 
- The FHE enablement at the underlying protocol level (node level) turns a perfect information game (i.e. blockchain due to the data transparency) to an imperfect information game hence to reach and maintain the Nash equilibrium. This will be particularly useful to prevent the malicious behaviors, often arise from AVS nodes. 
- Moreover, for a fair and resilient slashing mechanism, Verisense involves role game models at the fundamental node-level design. The three-party game includes Restaker (TVL supplier), Operator (Verisense AVS Node), and Resolver (Slasher) to prevent the “second-mover advantage.” FHE enablement allows Verisense to cleverly avoid many problems and build a functional slashing mechanism while maintaining fairness and resilience. Key issues addressed include:
  - The problem of convergence of bribe values due to the exposure of the Operator's bribery strategy for Restaker.
  - As a Resolver, the need to hide its veto of a slash's voting.
  - Front-running problems with MEVs.


Considering the recent developments in restaking, EigenLayer, as the pioneer in implementing and productizing the restaking concept, broke new ground and established LRT/restaking as a widely accepted method to enhance the trust of off-chain components. Now, VeriSense's innovation with FHE-enablement and VaaS technology marks another significant step forward, essential for realizing the true vision of decentralized security services.


