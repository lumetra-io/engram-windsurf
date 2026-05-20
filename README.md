# engram-windsurf

[Windsurf](https://windsurf.com) integration for [Engram](https://lumetra.io) — durable, explainable memory for Cascade, Windsurf's AI editor agent.

Adds six MCP tools to Cascade — `store_memory`, `query_memory`, `list_memories`, `list_buckets`, `delete_memory`, `clear_memories` — backed by the hosted Engram MCP server.

## Setup

### 1. Get an Engram API key

Sign up at <https://lumetra.io> — free tier, no card. You'll see an `eng_live_…` token in your dashboard.

### 2. Configure a BYOK provider key

Engram is bring-your-own-key end-to-end for the LLM that handles extraction and synthesis. Configure one provider at <https://lumetra.io/models>. DeepSeek is what we recommend — cheap and fast. Without a provider key, every `store_memory` / `query_memory` returns HTTP 412.

### 3. Add Engram to Windsurf's MCP config

Windsurf reads MCP servers from `~/.codeium/windsurf/mcp_config.json` (same path on macOS, Linux, and Windows). Create the file if it doesn't exist and merge in the `engram` block:

```json
{
  "mcpServers": {
    "engram": {
      "serverUrl": "https://mcp.lumetra.io/mcp/sse",
      "headers": {
        "Authorization": "Bearer eng_live_..."
      }
    }
  }
}
```

Restart Windsurf (or click **Refresh** in the Cascade MCP panel).

### Verify

Open the Cascade panel → top-right **MCPs** icon → `engram` should appear in your server list with a green status dot. Hovering shows the six available tools.

On first actual tool use in a conversation, Cascade asks once to approve the server.

## Tools exposed

| Tool | What it does |
|---|---|
| `store_memory(content, bucket?)` | Save a fact to a bucket (defaults to `"default"`). |
| `query_memory(question, bucket?)` | Hybrid retrieval + synthesized answer with citations. |
| `list_memories(bucket, limit?)` | Newest-first list of memories in a bucket. |
| `list_buckets(limit?, offset?)` | Paginated list of all buckets in your tenant. |
| `delete_memory(memory_id, bucket)` | Remove a single memory. |
| `clear_memories(bucket)` | Empty a bucket. Destructive. |

## Source & contact

- Source: <https://github.com/lumetra-io/engram-windsurf>
- Issues: <https://github.com/lumetra-io/engram-windsurf/issues>
- Lumetra: <https://lumetra.io> · <support@lumetra.io>

## License

MIT — Lumetra
