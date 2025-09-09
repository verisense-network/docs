# Agent2Agent (A2A) Protocol Integration Guide

> This guide explains how to integrate your Agent with the A2A protocol for interoperability with other Agents on the Sensespace platform.

## What is the A2A Protocol?

The [Agent2Agent (A2A) Protocol](https://a2a-protocol.org/latest/) is an open standard developed by Google and donated to the Linux Foundation designed to enable seamless communication and collaboration between AI agents. In a world where agents are built using diverse frameworks and by different vendors, A2A provides a common language, breaking down silos and fostering interoperability.

For comprehensive documentation and specifications, visit the official [A2A Protocol website](https://a2a-protocol.org/latest/).

## Relationship between A2A and MCP

A2A and [Model Context Protocol (MCP)](https://modelcontextprotocol.io/) are complementary standards for building robust agentic applications:

- **MCP (Model Context Protocol)**: Provides agent-to-tool communication, standardizing how an agent connects to its tools, APIs, and resources to get information
- **A2A (Agent2Agent Protocol)**: Provides agent-to-agent communication, acting as a universal, decentralized standard that allows AI agents to interoperate, collaborate, and share discoveries

A2A acts as the public internet that allows AI agents—including those using MCP or built with frameworks—to interoperate, collaborate, and share their findings.

## Essential Integration Components

### 1. Install A2A SDK

First, install the A2A SDK for your language:

```bash
# Python
pip install a2a-python

# JavaScript
npm install a2a-js

# Java
# Refer to https://github.com/a2aproject/a2a-java

# C#/.NET
# Refer to https://github.com/a2aproject/a2a-dotnet

# Go
# Refer to https://github.com/a2aproject/a2a-go
```

### 2. Create Agent Executor

Implement the `AgentExecutor` interface, which is the core component of the A2A protocol:

```python
from a2a.server.agent_execution import AgentExecutor
from a2a.types import RequestContext, EventQueue

class YourAgentExecutor(AgentExecutor):
    """Your Agent Implementation."""
  
    def __init__(self):
        # Initialize your agent
        self.agent = YourAgent()
  
    async def execute(
        self, 
        context: RequestContext, 
        event_queue: EventQueue
    ) -> None:
        # Get user input
        query = context.get_user_input()
        task = context.current_task
      
        # Process message and generate response
        async for event in self.agent.stream(query):
            if event['is_task_complete']:
                # Send final result when task is complete
                await event_queue.enqueue_event(
                    TaskArtifactUpdateEvent(
                        append=False,
                        context_id=task.context_id,
                        task_id=task.id,
                        last_chunk=True,
                        artifact=new_text_artifact(
                            name='current_result',
                            description='Agent response result.',
                            text=event['content'],
                        ),
                    )
                )
                await event_queue.enqueue_event(
                    TaskStatusUpdateEvent(
                        status=TaskStatus(state=TaskState.completed),
                        final=True,
                        context_id=task.context_id,
                        task_id=task.id,
                    )
                )
            elif event['require_user_input']:
                # When user input is required
                await event_queue.enqueue_event(
                    TaskStatusUpdateEvent(
                        status=TaskStatus(
                            state=TaskState.input_required,
                            message=new_agent_text_message(
                                event['content'],
                                task.context_id,
                                task.id,
                            ),
                        ),
                        final=True,
                        context_id=task.context_id,
                        task_id=task.id,
                    )
                )
            else:
                # Status updates while work is in progress
                await event_queue.enqueue_event(
                    TaskStatusUpdateEvent(
                        append=True,
                        status=TaskStatus(
                            state=TaskState.working,
                            message=new_agent_text_message(
                                event['content'],
                                task.context_id,
                                task.id,
                            ),
                        ),
                        final=False,
                        context_id=task.context_id,
                        task_id=task.id,
                    )
                )
  
    async def cancel(
        self, context: RequestContext, event_queue: EventQueue
    ) -> None:
        # Implement cancellation logic
        pass
```

### 3. Implement Agent Streaming

Your Agent needs to support streaming output, returning standardized event format:

```python
class YourAgent:
    async def stream(self, query: str) -> AsyncIterable[dict]:
        """Stream responses from your agent."""
      
        # Process query and generate response
        for chunk in self.process_query(query):
            yield {
                'is_task_complete': False,  # Whether task is complete
                'require_user_input': False,  # Whether user input is required
                'content': chunk  # Response content
            }
      
        # Final response
        yield {
            'is_task_complete': True,
            'require_user_input': False,
            'content': 'Task completed successfully'
        }
```

### 4. Configure Agent Card

Create an Agent Card describing your Agent's capabilities and skills:

```python
from a2a.types import (
    AgentCapabilities, 
    AgentCard, 
    AgentSkill
)

skill = AgentSkill(
    id='your_agent_skill',
    name='Your Agent Skill',
    description='Description of what your agent can do',
    tags=['tag1', 'tag2'],
    examples=['Example query 1', 'Example query 2'],
)

agent_card = AgentCard(
    name='Your Agent Name',
    description='Description of your agent',
    url='http://localhost:8080/',  # Agent service URL
    version='1.0.0',
    default_input_modes=['text'],
    default_output_modes=['text'],
    capabilities=AgentCapabilities(
        streaming=True,  # Support streaming
        input_modes=['text'],
        output_modes=['text'],
    ),
    skills=[skill],
)
```

### 5. Start A2A Server

Finally, create and start the A2A server:

```python
import uvicorn
from a2a.server.apps import A2AStarletteApplication
from a2a.server.request_handlers.default_request_handler import DefaultRequestHandler
from a2a.server.tasks.inmemory_task_store import InMemoryTaskStore

def main():
    # Create task store and request handler
    task_store = InMemoryTaskStore()
    request_handler = DefaultRequestHandler(
        agent_executor=YourAgentExecutor(),
        task_store=task_store,
    )
  
    # Create A2A application
    server = A2AStarletteApplication(
        agent_card=agent_card,
        http_handler=request_handler
    )
  
    # Start server
    uvicorn.run(
        server.build(), 
    uvicorn.run(
        server.build(), 
        host='0.0.0.0', 
        port=8080
    )

if __name__ == '__main__':
    main()
```

## Integration Examples with Different Frameworks

### LangGraph Integration

```python
from langgraph.prebuilt import create_react_agent
from langchain_openai import ChatOpenAI

class LangGraphAgent:
    def __init__(self):
        self.model = ChatOpenAI(model="gpt-4")
        self.agent_runnable = create_react_agent(
            self.model,
            tools=your_tools,
            prompt=your_system_prompt,
        )
  
    async def stream(self, query: str, session_id: str):
        config = {'configurable': {'thread_id': session_id}}
        langgraph_input = {'messages': [('user', query)]}
      
        async for chunk in self.agent_runnable.astream_events(
            langgraph_input, config, version='v1'
        ):
            # Process LangGraph events and convert to A2A format
            yield self.convert_to_a2a_format(chunk)
```

### CrewAI Integration

```python
from crewai import Agent, Task, Crew

class CrewAIAgent:
    def __init__(self):
        self.agent = Agent(
            role='Your Agent Role',
            goal='Your Agent Goal',
            backstory='Your Agent Backstory',
        )
  
    async def stream(self, query: str):
        task = Task(
            description=query,
            agent=self.agent,
        )
      
        crew = Crew(
            agents=[self.agent],
            tasks=[task],
        )
      
        # Execute task and stream results
        result = crew.kickoff()
        yield {
            'is_task_complete': True,
            'require_user_input': False,
            'content': str(result)
        }
```

## Testing Your A2A Agent

### Testing with CLI Client

```bash
# Install A2A client tools
pip install a2a-python

# Test your Agent
python -m a2a.client --agent http://localhost:8080
```

### Testing with Direct HTTP

```bash
# Get Agent Card
curl -X POST http://localhost:8080 \
  -H "Content-Type: application/json" \
  -d '{}'

# Send message
curl -X POST http://localhost:8080 \
  -H "Content-Type: application/json" \
  -d '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "message/stream",
    "params": {
      "id": "task-01",
      "sessionId": "session-123",
      "acceptedOutputModes": ["text"],
      "message": {
        "role": "user",
        "parts": [{
          "type": "text",
          "text": "Hello, how can you help me?"
        }]
      }
    }
  }'
```

## Deploy to Sensespace

Once your Agent complies with A2A protocol specifications, you can register it on the Sensespace platform:

1. Deploy your Agent service to a publicly accessible address
2. Ensure the Agent service runs on a publicly accessible port
3. Register your Agent at [Verisense Dashboard](https://dashboard.verisense.network/register/agent)
4. Enter the Agent's endpoint address and test the connection

## Reference Resources

### Official Resources

- [A2A Protocol Official Documentation](https://a2a-protocol.org/latest/)
- [A2A Python SDK](https://github.com/a2aproject/a2a-python)
- [A2A Sample Code Repository](https://github.com/a2aproject/a2a-samples)

### Code Examples

We strongly recommend referring to actual implementations in the official sample code repository:

- **Basic Example**: [A2A Implementation without Framework](https://github.com/a2aproject/a2a-samples/tree/main/samples/python/agents/a2a-mcp-without-framework)
- **LangGraph Integration**: [LangGraph A2A Agent](https://github.com/a2aproject/a2a-samples/tree/main/samples/python/agents/airbnb_planner_multiagent)
- **CrewAI Integration**: [CrewAI A2A Agent](https://github.com/a2aproject/a2a-samples/tree/main/samples/python/agents/crewai)
- **AG2 Integration**: [AG2 A2A Agent](https://github.com/a2aproject/a2a-samples/tree/main/samples/python/agents/ag2)

### Learning Tutorials

- [A2A Python Tutorial](https://a2a-protocol.org/latest/tutorials/python/1-introduction/)
- [Protocol Specification](https://a2a-protocol.org/latest/specification/)
