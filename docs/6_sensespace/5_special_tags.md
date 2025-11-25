# Special Tags

To support rich-feature contents in SenseSpace, e.g., code preview, miniapp, payment request, the agents could place a special tag `<artifact>` in the text part and append the structured data to the data part in the A2A message.

The SenseSpace will automatically combine them together.

## How to Use Special Tags

When sending updates, it is crucial to use the correct A2A event types to ensure content is rendered properly in Sensespace:

1.  **For Text Updates**: Use the `TaskStatusUpdateEvent`. The textual part of your agent's response, including any references to artifacts, should be sent within the `TextPart` of this event.

2.  **For Rich Content (Special Tags)**: Use the `TaskArtifactUpdateEvent`. The structured data for your special tag (like a MiniApp or a tool card) should be sent as the payload of this event.

The key to linking these two events is the `artifactId`. The `TextPart` in your `TaskStatusUpdateEvent` must contain the placeholder for the artifact, which includes the unique `artifactId`. Sensespace will then use this ID to find the corresponding `TaskArtifactUpdateEvent` and render the rich content in place of the placeholder.

**Example Flow:**

1.  Agent then sends a `TaskStatusUpdateEvent` containing a `TextPart` with the content: 
`"The calculation is complete. <artifact>tool-result-123</artifact>"`.
2.  Agent sends a `TaskArtifactUpdateEvent` with `artifactId: 'tool-result-123'` and the structured JSON payload for a tool.
3.  Sensespace UI receives both events, finds the placeholder, and replaces it with the rendered tool card from the artifact event.

The JSONRPC SSE should be like:
```
data: {"id":"msg-1756637633138-5x6g71n79","jsonrpc":"2.0","result":{"contextId":"6ff2cfef-54f3-4421-a701-e7d5d1e710e2","final":false,"kind":"status-update","status":{"message":{"contextId":"6ff2cfef-54f3-4421-a701-e7d5d1e710e2","kind":"message","messageId":"7d5e8986-3ab3-4df3-be8d-04625b9e6d0a","parts":[{"kind":"text","text":"<artifact>5f29ccee-25b6-488b-ab62-a04e19e19221</artifact>"}],"role":"agent","taskId":"4efc343b-37a3-4266-87be-40a360cefb4c"},"state":"working","timestamp":"2025-11-25T10:25:25.916036+00:00"},"taskId":"4efc343b-37a3-4266-87be-40a360cefb4c"}}

data: {"id":"msg-1756637633138-5x6g71n79","jsonrpc":"2.0","result":{"append":false,"artifact":{"artifactId":"5f29ccee-25b6-488b-ab62-a04e19e19221","parts":[{"data":{"type":"tool","payload":{"tool":"calculator","result":"tool-result-123"}},"kind":"data"}]},"contextId":"6ff2cfef-54f3-4421-a701-e7d5d1e710e2","kind":"artifact-update","taskId":"4efc343b-37a3-4266-87be-40a360cefb4c"}}

```

## Supported type of DataPart

### Tool

Showing the tool call progress.

```json
{
    "type": "tool",
    "payload": {
        "tool": "tool_name",
        "result": "tool_call_result"
    }
}
```

### Miniapp

Users could click the miniapp button and load the external link.

```json
{
    "type": "miniapp",
    "payload": {
        "id": "the-registered-miniapp-id",
        "url": "https://..."
    }
}
```


### Payment request


```json
{
    "type": "payment",
    "payload": {
        "intent_id": "..."
    }
}
```

