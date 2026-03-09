
# Data Layer – Step-by-Step Client–Server Interaction

This section walks through a complete MCP client–server interaction using **JSON-RPC 2.0** messages.

We’ll cover:
1. Lifecycle management (initialization)
2. Tool discovery
3. Tool execution
4. Notifications

The goal is to show how MCP primitives operate in practice.

```
User            LLM              MCP Host              MCP Server
 |               |                    |                     |
 |--- Prompt --->|                    |                     |
 |               |--- Prompt -------->|                     |
 |               |                    |--- initialize ----->|
 |               |                    |<-- initialize ------|
 |               |                    |--- initialized ---->|
 |               |                    |                     |
 |               | (reasoning)        |                     |
 |               |--- tool intent --->|                     |
 |               |                    |--- tools/list ----->|
 |               |                    |<-- tools/list ------|
 |               |                    |                     |
 |               |--- tool call ----->|                     |
 |               |                    |--- tools/call ----->|
 |               |                    |<-- result ----------|
 |               |<-- tool result ----|                     |
 |               | (final reasoning)  |                     |
 |<-- Answer ----|                    |                     |
```

### 1️⃣ Initialization (Lifecycle Management)

Every MCP connection begins with a **capability negotiation handshake**.
The client sends an `initialize` request to:
- Negotiate protocol version
- Advertise supported capabilities
- Exchange identity information

```
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "initialize",
  "params": {
    "protocolVersion": "2025-06-18",
    "capabilities": {
      "elicitation": {}
    },
    "clientInfo": {
      "name": "example-client",
      "version": "1.0.0"
    }
  }
}
```

### Initialize Response (Example)

```
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "protocolVersion": "2025-06-18",
    "capabilities": {
      "tools": { "listChanged": true },
      "resources": {}
    },
    "serverInfo": {
      "name": "example-server",
      "version": "2.1.0"
    }
  }
}
```

### Understanding the Initialization Exchange

The initialization process serves several critical purposes:

#### ✅ Protocol Version Negotiation

The `protocolVersion` field ensures compatibility.
If the client and server cannot agree on a mutually supported version, the connection must terminate.
This prevents subtle runtime errors later.

#### ✅ Capability Discovery

The `capabilities` object allows both parties to declare supported features.

**Client capability example:**
```
"elicitation": {}
```

Meaning:
- The client supports interactive user prompts (`elicitation/create`)

**Server capability examples:**
```
"tools": { "listChanged": true }
```

Meaning:

- The server supports tools
- It can send `tools/list_changed` notifications when its tool list updates
```
"resources": {}
```

Meaning:
- The server supports resource-related operations
This avoids unsupported operations and enables feature-aware behavior.

### ✅ Identity Exchange

The `clientInfo` and `serverInfo` objects help with:

- Debugging
- Logging
- Version tracking
- Compatibility diagnostics

## Initialization Completion

After successful initialization, the client signals readiness:
```
{
  "jsonrpc": "2.0",
  "method": "notifications/initialized"
}
```

This is a **notification** (no `id` field), meaning no response is expected.

# 2️⃣ Tool Discovery (Primitives)

Once initialized, the client can discover available tools.

This is done via:

```
tools/list
```

#### Tools List Request

```
{
  "jsonrpc": "2.0",
  "id": 2,
  "method": "tools/list"
}
```

Note:
- No parameters are required
- The request is intentionally simple

#### Tools List Response (Example)

```
{
  "jsonrpc": "2.0",
  "id": 2,
  "result": {
    "tools": [
      {
        "name": "weather_current",
        "title": "Get Current Weather",
        "description": "Returns the current weather for a location.",
        "inputSchema": {
          "type": "object",
          "properties": {
            "location": { "type": "string" },
            "units": { "type": "string", "enum": ["metric", "imperial"] }
          },
          "required": ["location"]
        }
      }
    ]
  }
}
```

##### Understanding the Tool Metadata


| Property    | Value                                                                                                                                                                            | Meaning                                                                                                                                                                                             |
| ----------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| name        | *weather_current*                                                                                                                                                                | A unique identifier used when invoking the tool.                                                                                                                                                    |
| title       | *"Get Current Weather"*                                                                                                                                                          | A human-readable display name.                                                                                                                                                                      |
| description | *"Returns the current weather for a location."*                                                                                                                                  | Explains when and why to use the tool.                                                                                                                                                              |
| inputSchema | *"inputSchema": {  <br>"type": "object",  <br>"properties": {  <br>"location": { "type": "string" },  <br>"units": { "type": "string", "enum": ["metric", "imperial"] }  <br>},* | A JSON Schema definition of expected parameters. This enables:<br><br>- Type validation<br>    <br>- Automatic UI generation<br>    <br>- LLM tool-call validation<br>    <br>- Clear documentation |
| required    | *"location"*                                                                                                                                                                     | required input parameter for request                                                                                                                                                                |

# Putting It All Together

The full lifecycle looks like this:
1. **Initialize** → Negotiate capabilities
2. **Initialized notification** → Ready state
3. **tools/list** → Discover functionality
4. **tools/call** → Execute operations
5. **notifications/** → Receive real-time updates

This structured, JSON-RPC-based data layer ensures:
- Predictable communication
- Type safety
- Extensibility
- Backward compatibility
- Clear separation of responsibilities