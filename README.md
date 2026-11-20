# B-AI Code

A coding agent for VS Code that runs on a local model (Ollama) or a frontier API. The harness stays the same;
the model is a setting.

## Features

**Models**
- Works with local models through Ollama, Claude through the Anthropic API, and any OpenAI-compatible endpoint.
- Finds Ollama on your PC by itself and lists the models that can call tools.
- Switch model mid-chat: the conversation continues with the same tools and rules.
- Context size and thinking mode are set automatically for each model.

**Coding**
- Reads, searches and edits files in your workspace, then runs your build and tests to check its work.
- Every changed file has a **diff** button, and **Undo all** restores everything.
- Reads project instructions from `BALA-AI.md` or `AGENTS.md` at the repository root.

**Safety**
- Every shell command asks for approval, except prefixes you allowlist (such as `npm test`).
- Dangerous commands are always blocked: `sudo`, `rm -rf /`, force-push, `git reset --hard`, `curl … | bash`.
- `.git/`, `.env` files and the agent's own policy files are protected. Nothing outside the workspace is touched.
- Per-repository rules in `.bala-ai/policy.json`: blocked commands, protected paths, auto-approved commands.

**Built for small context windows**
- **Skills:** instructions in a `SKILL.md` that load only when a task needs them. Skills written for Claude Code
  and Codex (`.claude/skills`, `.agents/skills`) work unchanged.
- **Helper:** a read-only second agent with its own context. It digs through large logs or many files and returns
  only a short answer.
- **Compaction:** older tool output is condensed automatically before the window fills.
- **Loop detection:** repeating the same call three times triggers a change of approach.

**Tools (MCP)**
- Connects to any [MCP](https://modelcontextprotocol.io) server over stdio or streamable HTTP: GitHub, issue
  trackers, mail, documentation.
- Reads `.bala-ai/mcp.json` and the `.mcp.json` that Claude Code uses.
- Tool schemas load on demand, so many servers fit in a 32K window.
- Every MCP call asks for approval unless allowlisted. Tokens stay in environment variables, never in logs.

**Privacy and audit**
- No telemetry. Prompts and code go only to the model you pick.
- API keys are kept in VS Code's secret storage.
- Every request, tool call and token count is logged to `.bala-ai/logs/`.

**Benchmark**
- A built-in exam compares harnesses on the same local model: 23 embedded-C tasks and 12 web-development tasks,
  graded by hidden tests in a sandbox.
- Runs B-AI against Claude Code, Codex CLI, OpenCode, Pi, Qwen Code, Hermes, DeepSeek Harness, Aider and
  Harness², and builds a results page with confidence intervals.

## Get started

1. Install **B-AI Code** from the [VS Code Marketplace](https://marketplace.visualstudio.com/items?itemName=bala-ai.bala-ai-code), or search for it in the Extensions view.
2. Open a folder and click the B-AI icon in the sidebar.
3. Pick a model in the dropdown. For Claude, it asks for your API key the first time.

The panel has three tabs: **Chat**, **How to** (a short guide) and **Settings**. Every setting is optional.

## Add an MCP server

Run **B-AI: Add MCP Server**, or write `.bala-ai/mcp.json`:

```json
{
  "mcpServers": {
    "github": {
      "url": "https://api.githubcopilot.com/mcp/",
      "headers": { "Authorization": "Bearer ${env:GITHUB_TOKEN}" },
      "description": "GitHub issues, pull requests and code search"
    }
  }
}
```

## License

Free to install and use. The source code is private and not licensed for reuse. See the licence file included with the extension.
