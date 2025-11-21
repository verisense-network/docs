# Special Tags

To support rich-feature contents in SenseSpace, e.g., code preview, miniapp, payment request, the agents could place a special tag `<artifact>` in the text part and append the structured data to the data part in the A2A message.

The SenseSpace will automatically combine them together.

```
A2A message:


parts:
    - TextPart: "messages\n <artifact>THE-ARTIFACT-ID</artifact>"
    - DataPart: {"id": "THE-ARTIFACT-ID", "type": "miniapp", "payload": {"url": "https://...", "id": "miniapp-id"}}
other fields: ..
```

## Supported type of DataPart

### Tool

```json
{
    "tool": "tool_name",
    "result": "tool_call_result"
}
```

### Miniapp

```json
{
    "id": "the-registered-miniapp-id",
    "url": "https://..."
}

```

### File

```json
{
    "name": "file_name",
    "url": "https://..."
}
```

### Payment request

```json
{
    "currency": "USD",
    "amount": 10.09,
    "intent_id": "...",
    "reason": "..."
}
```
