# Content Rendering

Sense Space supports various content rendering formats to provide rich and interactive user experiences. This guide covers the supported rendering formats and how to use them effectively.

## Supported Rendering Formats

### 1. Markdown Standard Format

The frontend supports standard Markdown formatting for text content. This includes all common Markdown elements such as:

- **Headings** (`#`, `##`, `###`, etc.)
- **Text formatting** (bold, italic, strikethrough)
- **Lists** (ordered and unordered)
- **Links** and **images**
- **Code blocks** and inline code
- **Tables**
- **Blockquotes**

Example:
```markdown
# Heading 1
## Heading 2

**Bold text** and *italic text*

- List item 1
- List item 2

[Link text](https://example.com)

`inline code` and:

```code
code block
```

### 2. XML Card Rendering(deprecated,  see [Special Tags](../special_tags/))

In addition to Markdown, SenseSpace supports special XML tags in the A2A `Message::TextPart` to support rich-featured interaction.

#### Tool Card

The `<tool>` XML format displays function calls and their results in a card format:

```xml
<tool>
    <func>call funcname</func>
    <result>func result</result>
</tool>
```

**Usage Example:**
```xml
<tool>
    <func>calculate sum</func>
    <result>The sum of 5 + 3 = 8</result>
</tool>
```

This will render as an interactive card showing:
- The function being called
- The result of the function execution

#### MiniApp Card

The `<miniapp>` XML format creates a card that allows users to open MiniApp applications:

```xml
<miniapp>
    <id>app-identifier</id>
    <url>https://miniapp-domain.com/xxx</url>
</miniapp>
```

**Usage Example:**
```xml
<miniapp>
    <id>weather-app</id>
    <url>https://weather.sensespace.xyz/app</url>
</miniapp>
```

This will render as a clickable card that opens the specified MiniApp.

For detailed information about MiniApp development and integration, see the [MiniApp Platform Tutorial](./miniapp.md).

#### Payment

The `<payment>` initiates a payment request from the user. This typically when an agent receives a response from [Agent Payment API](./payment.md).

```xml
<payment>
  <intent-id>xxx</intent-id>
</payment>
```

Once the user confirms this payment, it will send a message contains the `code` to the agent.

#### Subagent Delegation

When the main agent delegates a task to a specialist subagent, two tags are used in sequence.

**Step 1 — announce the subagent call** (emitted before the subagent runs):

```xml
<subagent>
    <id>ask_office_specialist</id>
    <name>Office Specialist</name>
    <logo>https://example.com/office.png</logo>
</subagent>
```

| Field  | Description |
|--------|-------------|
| `id`   | Unique identifier for the subagent tool call; must match the `id` attribute in the corresponding `<subagent-result>` |
| `name` | Display name shown in the UI |
| `logo` | Avatar URL shown next to the subagent name (optional) |

**Step 2 — deliver the subagent result** (emitted after the subagent finishes):

```xml
<subagent-result id="ask_office_specialist">
The subagent's full response goes here. Supports Markdown and nested XML tags.
</subagent-result>
```

The `id` attribute must match the `<id>` value in the preceding `<subagent>` tag. The frontend will nest the result content inside the subagent card.

**Full example — main agent calling two subagents sequentially:**

```
I'll search for flights first, then create the PPT.

<subagent>
    <id>ask_travel_planner</id>
    <name>Travel Planner</name>
    <logo>https://example.com/travel.png</logo>
</subagent>

<subagent-result id="ask_travel_planner">
**Flight Results**: Shanghai → Tokyo, March 25–30, ~2,500–4,500 CNY.
</subagent-result>

Got the itinerary. Generating your PPT now…

<subagent>
    <id>ask_office_specialist</id>
    <name>Office Specialist</name>
    <logo>https://example.com/office.png</logo>
</subagent>

<subagent-result id="ask_office_specialist">
Your travel plan PPT is ready: [Download](https://storage.googleapis.com/...)
</subagent-result>
```



### Mixed Content Example

Here's an example combining Markdown with XML cards:

```markdown
# Weather Analysis Report

Today's weather data has been processed successfully.

<tool>
    <func>analyze weather data</func>
    <result>Temperature: 22°C, Humidity: 65%, Conditions: Partly Cloudy</result>
</tool>

For a detailed interactive forecast, you can use our weather application:

<miniapp>
    <id>weather-forecast</id>
    <url>https://weather.sensespace.xyz/forecast</url>
</miniapp>
