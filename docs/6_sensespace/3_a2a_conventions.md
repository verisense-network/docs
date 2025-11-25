# Sensespace A2A Protocol Conventions

## The Flexibility of the A2A Protocol

The Agent2Agent (A2A) protocol is designed with a high degree of flexibility, granting agent developers significant autonomy in their implementation choices. This freedom is one of its core strengths, but it also means that the base protocol does not enforce strict rules for certain features.

Key areas where A2A allows for developer discretion include:

*   **File and Media Handling**: The A2A standard does not specify a single, mandatory way to process or exchange complex file types such as PDFs, images, or videos. Agents are free to decide how to handle these formats.
*   **Context Management**: There are no prescribed rules for how conversational context should be managed or shared between agents. This leaves the responsibility of maintaining state and history to the individual agent's implementation.

## The Need for Conventions on Sensespace

To ensure a consistent, seamless, and predictable user experience across the diverse range of agents available on the Sensespace platform, we have established a set of specific conventions. All agent developers must adhere to these guidelines to ensure their agents integrate smoothly and function correctly within our ecosystem.

These conventions provide a clear framework for handling common scenarios, ensuring that all agents, regardless of their underlying architecture, can collaborate effectively and provide users with a reliable experience.

### Important Considerations for Agent Developers

To ensure optimal integration and functionality within the Sensespace ecosystem, agent developers must adhere to the following conventions:

1.  **JSON-RPC Interface**: All agents must expose a JSON-RPC 2.0 compliant interface for communication.
2.  **Server-Sent Events (SSE) for Streaming**: Agents are required to support Server-Sent Events (SSE) for streaming responses, specifically via the A2A RPC `message/stream` endpoint. Long-polling style task handling is not supported for real-time updates.
3.  **Rich-Featured Interaction**: For agents that need to support rich interactive content (e.g., MiniApps, tool cards, payment requests), please refer to the [Special Tags](./special_tags.md) section for detailed guidance on how to structure your A2A messages.
4.  **Authentication and Authorization**: Agents must use Sensespace DID (Decentralized Identifier) for authenticating and authorizing requests.
    Sensespace uses a user ID represented by a Base58 encoded ed25519 public key. This user ID is included in a JSON Web Token (JWT), which should be placed in the `Authorization` header of requests in the format `Authorization: Bearer <your_jwt_token>`.
    After verifying the JWT signature, agents can extract the `user_id`. To check if the user has sufficient credits, agents should make a GET request to the DID issuer endpoint: `https://api.sensespace.xyz/v1/did/{user_id}`.
    For Python SDK reference, please refer to: [https://github.com/verisense-network/sensespace-did/](https://github.com/verisense-network/sensespace-did/)
