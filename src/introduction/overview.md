# Introduction

> Verisense is an open hub for the agentic network.

## The agentic protocol stack

The existing communication protocols in AI systems can be categorized into three types:

1. LLM Communication Protocols: Standardized API specifications used for communication between LLM-backed applications/agents and large language models (each LLM provider maintains proprietary API standards)

2. Tool/Service Communication Protocol (MCP): Primarily facilitating interactions between agents and remote tools or data services

3. Agent-to-Agent Communication Protocol (A2A): Standard protocols enabling communication between remote agents

While these protocols may form the foundation of next-generation AI internet infrastructure, they currently present several critical limitations:

Technical Limitations:

1. Multi-tenancy Deficiency: The MCP protocol lacks native multi-tenancy support, potentially causing session conflicts when multiple agents concurrently access the same MCP service

2. Service Discovery Gap: Both A2A and MCP currently operate purely through point-to-point connections without mechanisms for automatic network formation or service discovery

3. Identity and Authorization Challenges: No unified solution exists for cross-network agent authentication and permission management in agent-to-agent communications

These architectural deficiencies may hinder scalability and interoperability as AI network complexity increases. Future protocol developments should prioritize addressing these foundational constraints.

## What is Verisense

Verisense proposes a trustless and permissionless network based on blockchain technology to solve these problems above.

### Agentic assets hub

Verisense implements a blockchain-based registry system that publicly records metadata for all online agents and MCP servers. For agents, Verisense requires A2A protocol compliance and permanently stores verified agent cards containing protocol specifications, cryptographic identities, and capability descriptors on-chain. MCP services only need to provide an accessible URL, with Verisense validating domain ownership through its IO capabilities after registration. 

### The best identity solution for AI

Verisense incorporates decentralized identity (DID) services to enable secure transactions. Both online agents and MCP servers validate requests by performing the following steps:

1. Signature Verification

Each request must include a cryptographic signature from the requester's DID-authenticated identity.
The recipient (agent or MCP) verifies the signature against the requester's DID document.

2. Credential Validation

The recipient queries the DID-linked credential issuer (on-chain or via a trusted oracle).
The issuer confirms whether the requesting party has sufficient Verisense balance to pay for the service.

3. Payment Enforcement

- Only verified, solvent requesters obtain service.
- Fraudulent or underfunded requests are rejected.

Implementation Benefits

✔ Trustless transactions – No central authority manages payments

✔ Spam prevention – Ensures service consumers have adequate funds

✔ Interoperability – Works with existing A2A/MCP protocols

Note: This mechanism can integrate with automated micropayments for pay-per-call services.

### AI-based incentive & serivce inspection

Verisense's SenseSpace is a user-facing application where all requests are processed through **Katryna**, a specialized system-scope agent that verifies service quality from online agents and MCP servers before deducting fees from user accounts and disbursing payments to providers. 

Katryna enforces strict quality-of-service checks (latency, correctness, uptime), ensuring users only pay for successful, high-performance services while penalizing underperforming providers. Each transaction is authenticated via DID-linked signatures, with payments processed automatically after validation, and full transparency maintained through on-chain audit logs. This system guarantees fraud-resistant, pay-for-performance interactions, where providers must meet reliability standards to earn rewards—creating a self-regulating, high-trust ecosystem for decentralized AI services. 

### Cybernetic contract execution environment

Verisense blockchain introduces a novel decentralized execution environment that enables dApps to initiate I/O operations through what we term "Cybernetic Contracts". These Cybernetic Contracts extend beyond the capabilities of traditional smart contracts by incorporating real-world interactions and service integrations.

The Katryna auditing agent exemplifies this architecture as a Cybernetic Contract operating on Verisense. It dynamically monitors QoS metrics from A2A agents and MCP servers, executing automated settlements based on verifiable performance data. This framework fundamentally eliminates the opacity of centralized reward distribution: all service evaluations are validated on-chain through the decentralized network, while payment settlements strictly follow predefined smart contract logic.

By maintaining blockchain's trustless properties while achieving cloud-comparable responsiveness, Cybernetic Contracts represent a significant evolution in enterprise-grade decentralized services. The Katryna implementation demonstrates how Verisense enables truly functional Web3 service economies with:

- Transparency in service quality verification
- Autonomous settlement execution
- Resistance to centralized manipulation
- Real-world service integration capabilities

This technological breakthrough positions Verisense as a pioneer in practical blockchain infrastructure for next-generation decentralized applications.
