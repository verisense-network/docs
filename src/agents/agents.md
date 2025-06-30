## Building Agents on the Verisense Network

### What is an Agent?

On the Verisense Network, an **Agent** is an autonomous, on-chain entity designed to perform complex tasks by reasoning, interacting with its environment, and coordinating with other systems. It is not just a static smart contract but a dynamic, intelligent program defined by a powerful core formula:

> **Agent = Context + Tools + LLM**

This architecture allows for the creation of sophisticated agents that can manage digital assets, interact with off-chain data, and collaborate in a decentralized manner. Let's break down each component in detail.

---

### 1. Context: The Agent's State and Memory

The **Context** is the agent's memory and operational awareness. It provides the stateful foundation that allows an agent to go beyond simple, one-off transactions and engage in meaningful, multi-step tasks. This includes:

* **Goals & Objectives:** The primary mission the agent is programmed to achieve (e.g., "maximize staking rewards across three protocols" or "execute a trade when market volatility exceeds a 5% threshold").
* **Knowledge Base:** A repository of specialized information or historical data the agent can draw upon to make more informed decisions.
* **Session Data:** The real-time, short-term memory of its current task, including conversation history, user preferences, and the status of ongoing operations.
* **Configuration:** The agent's unique settings, such as its risk tolerance, security parameters, and credentials for accessing specific tools.

Verisense provides a flexible, hybrid approach to storing this context, balancing the trade-offs between security, cost, and accessibility:

* **Off-Chain Storage:** For large volumes of non-critical data, context can be stored in traditional off-chain databases or decentralized storage networks (like IPFS). This is ideal for knowledge bases or extensive logs where on-chain immutability is not required.
* **On-Chain Storage via Nucleus:** For critical state information—such as goals, configurations, or the final outcome of a task—agents can leverage **Verisense Nucleus**. Nucleus acts as a secure, on-chain storage and messaging layer, ensuring that the agent's core context is immutable, transparent, and auditable.

### 2. Tools: The Agent's Arms and Hands

**Tools** (or functions) are what allow an agent to break out of the blockchain's sandbox and interact with the wider digital world. They are the practical capabilities the agent can call upon to execute the steps of its plan. Verisense supports a rich ecosystem of both on-chain and off-chain tools:

* **Off-Chain Tools (The MCP Model):** Agents can securely access any external resource via off-chain tools, similar to the Multi-Party Computation (MCP) model. This includes:
    * **HTTP APIs:** For querying web services, fetching market data from exchanges, or interacting with social media platforms.
    * **Database Access:** Connecting to private or public databases to retrieve or store information.
    * **Executing Code:** Running scripts in various languages (e.g., Python, JavaScript) for complex data analysis or computation.

* **On-Chain Tools (The Nucleus Model):** Verisense Nucleus provides a suite of powerful on-chain tools that enable secure and decentralized operations:
    * **Threshold Signature Scheme (TSS):** This is a cornerstone feature for cross-chain operations. TSS allows a group of network nodes to collectively generate a signature to authorize a transaction on another blockchain (like Bitcoin or Ethereum) without ever forming a single private key. An agent can use this tool to manage assets or interact with dApps across different ecosystems seamlessly.
    * **http:** This tool is a built-in decentralized oracle that allows an agent to make secure HTTP requests and retrieve data from the off-chain world. An agent can submit a URL to the Nucleus layer, and the Verisense network validators will fetch the data, come to a consensus on the result, and deliver it back to the agent on-chain. This is critical for agents that need to react to real-world information, such as fetching current asset prices, weather data, or sports scores to trigger an on-chain action.
    * **timer:** This tool functions as a decentralized and reliable on-chain scheduler, often called a "cron service" for smart contracts. An agent can use the timer tool to register a function to be executed at a specific future time, block number, or on a recurring interval. This enables time-based strategies and automation, such as automatically harvesting staking rewards every 24 hours, executing a series of trades at set intervals (TWAP), or managing subscriptions.

### 3. LLM: The Reasoning Engine

The **LLM** is the agent's brain. It provides the cognitive power for reasoning, language comprehension, and, most importantly, **dynamic planning**. Within the Verisense framework, the LLM is responsible for:

* **Instruction Interpretation:** Understanding a user's high-level goals, even if they are ambiguous or complex.
* **Response Formulation:** Communicating its status, results, and actions in a clear, human-understandable manner.
* **Dynamic Interaction Planning:** This is where the agent's intelligence truly shines. Based on the user's goal, the LLM creates a step-by-step plan. It selects the right tool for each step, processes the output, and adjusts the plan based on the results. For example, if a web API call fails, the LLM can decide to try an alternative data source or notify the user.

### Fast Deploy an Agent

1.  Create an agent with the [A2A](https://github.com/verisense-network/a2a-samples/tree/main#) protocol.
2.  Register the agent in the Verisense [Dashboard](https://dashboard.verisense.network/).
3.  @ your agent in [Sensespace](https://space.verisense.network/chat).