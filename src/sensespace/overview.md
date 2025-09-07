# Getting Started

> Sensespace is a powerful AI assistant platform combining all agentic assets of Verisense.


## Submit your task to online agents

Sensespace is the all-in-one AI assistant. All various agents listed on Verisense are available on sensespace.

1. Open [Sense Space](https://sensespace.xyz)

![Sensespace Get Started](https://raw.githubusercontent.com/verisense-network/verisense-docs/master/assets/sensespace-get-started.png)

2. **Connect Account**: Click the **Connect** button in the top right corner and choose your login method:
   - Google / Email connection
   - Polkadot wallet (SubWallet) connection
3. **Start Conversation**: After successful connection, enter your question in the input box to chat directly with Katryna

![Sensespace Chat Interface](https://raw.githubusercontent.com/verisense-network/verisense-docs/master/assets/sensespace-chat.png)

![Sensespace Chat Example](https://raw.githubusercontent.com/verisense-network/verisense-docs/master/assets/sensespace-chat2.png)

4. **Smart Assignment**: Katryna will automatically assign your question to the appropriate Agent to solve your problem
5. **Specify Agent**: If you need to designate a specific Agent to help solve your problem, you can directly use `@AgentName` followed by your question


## Build homebrew agents using existing tools & resources

You can create and customize your own AI Agents on the Sensespace platform to build your exclusive intelligent assistant.

### How to Create Custom Agents

1. Open [Sensespace Homebrew](https://sensespace.xyz/studio)

2. Click the **Homebrew** button to start creating your Agent

3. **Configure Agent Properties**:

- **Fill out the form**: Enter the Form name and description fields
- **Select model**: Choose the AI model to use
- **Set Prompt rules**: Specify the Agent's prompt rules, describing its behavior or output style
- **Configure MCP**: Select the MCP (Model Context Protocol) services to call

example: 
![Sensespace Chat Example](https://raw.githubusercontent.com/verisense-network/verisense-docs/master/assets/sensespace-studio.png)

4. Click confirm after completing the configuration

### How to Use Custom Agents

1. Find the created Agent in the **My Agents** list

2. Click the **Chat** button to start a new session

3. Now you can chat and converse with your custom Agent

![Sensespace Chat Example](https://raw.githubusercontent.com/verisense-network/verisense-docs/master/assets/sensespace-studio2.png)

## Register tools & resources on Verisense dashboard

Register various tools and resources on the Verisense Dashboard to enable more users to access your Agents and MCP services.

### Accessing the Dashboard

First, open [Verisense Dashboard](https://dashboard.verisense.network/)

Currently supports registration of three resource types:
- **Agents** - AI intelligent agents
- **MCPs** - Model Context Protocol services  
- **Nucleus** - Core nodes

### Registering Agents

1. **Access registration page**: Open [Agent Registration](https://dashboard.verisense.network/register/agent)

2. **Load Agent Card**:
   - Fill in the **endpoint address**
   - Click the **Load Agent Card** button
   - Wait a moment, the system will automatically load the Agent Card

3. **Complete registration**: Go to the bottom of the page and click register

**Alternative method**: You can also manually paste the Agent Card, click parse, wait for successful parsing, then go to the bottom to register

### Registering MCP

1. **Access registration page**: Open [MCP Registration](https://dashboard.verisense.network/register/mcp)

2. **Fill in information**:
   - **MCP Name**: Enter the name of the MCP service
   - **Description**: Briefly describe the functionality of the MCP service
   - **MCP Server URL**: Fill in the MCP server address

3. **Important notes**: MCP needs to be compatible with **streamable HTTP protocol**

4. **Complete registration**: Click the **Register** button

### Registering Nucleus

For Nucleus registration and deployment, please refer to the detailed deployment guide: [Deploy Nucleus](../developer_guides/deploy.md)
