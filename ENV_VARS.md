# Environment Variables

Required and optional environment variables for the OpenCode multi-agent system.

## Required

| Variable | Description |
|----------|-------------|
| `CONTEXT7_API_KEY` | API key for context7 MCP server. Get from [context7.com](https://context7.com) |

## Optional

| Variable | Description | Default |
|----------|-------------|---------|
| `OPENCODE_MODEL` | Override default model | `opencode/claude-sonnet-4-5` |
| `OPENCODE_SMALL_MODEL` | Override small model | `opencode/claude-haiku-4-5` |
| `OPENCODE_DEBUG` | Enable debug logging | `false` |

## Setup

```bash
# Add to shell profile (~/.bashrc, ~/.zshrc, etc.)
export CONTEXT7_API_KEY="your-api-key-here"

# Or create .env file in project root
echo "CONTEXT7_API_KEY=your-api-key-here" > .env
```

## MCP Server Configuration

The config uses two MCP servers:

1. **context7** - Advanced context retrieval (requires `CONTEXT7_API_KEY`)
2. **gh_grep** - GitHub code search (no auth required)

Both configured with 10s timeout.
