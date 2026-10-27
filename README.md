<p align="center">
  <a href="https://docs.getport.app/build-with-ai/">
    <img src="https://docs.getport.app/cards/skills.png" alt="Port_ skills: connect your AI to your port" width="100%">
  </a>
</p>

<p align="center">
  <a href="https://getport.app"><b>getport.app</b></a> ·
  <a href="https://docs.getport.app/build-with-ai/">Build with AI</a> ·
  <a href="https://docs.getport.app/api/mcp/">MCP server</a> ·
  <a href="https://docs.getport.app/security/ai-access/">How it is kept safe</a>
</p>

# Port_ skills

Agent skills and a plugin for reading your own [Port_](https://getport.app) port from Claude
Code, Codex and anything else that speaks MCP or reads an Agent Skills folder: net worth,
holdings, DeFi, perps, prediction markets, NFTs, PnL, activity, the daily briefing and alerts,
with what is not counted and how old it is.

> **Read only.** Nothing here can move funds, sign a transaction or change a setting. Everything
> goes over `https://getport.app`, so this repository is a pointer, not a client.

<img src="https://docs.getport.app/cards/skills-works-with.png" alt="Works with Claude Code, Claude, ChatGPT, Codex, Cursor, VS Code, GitHub Copilot, Windsurf, Gemini CLI and OpenCode" width="100%">

## Install

<img src="https://docs.getport.app/cards/skills-steps.png" alt="Run one command, sign in at getport.app and press Allow, then ask" width="100%">

One command connects every coding agent on your machine to your port:

```bash
npx add-mcp@2.4.0 https://getport.app/mcp --name port -g
```

The first time an agent uses it, it opens getport.app: sign in, press Allow, and it is connected.
No key. Then teach them how Port_ answers:

```bash
npx skills add getport/skills -g
```

claude.ai, Claude Desktop and ChatGPT: add a custom connector with the address
`https://getport.app/mcp` and sign in when it asks.

Or both at once as a plugin. Claude Code:

```text
/plugin marketplace add getport/skills
/plugin install port@port
```

Codex:

```bash
codex plugin marketplace add getport/skills
codex plugin add port@port
```

It needs a Port_ Pro+ account or above. Scripts that call the REST
API use a key instead, made under Settings, API access; `port-api/SKILL.md` has how.

## What your AI can ask

<img src="https://docs.getport.app/cards/skills-tools.png" alt="The tools the MCP server offers, every one read only; refresh only when you allow it" width="100%">

Every answer leads with what the headline leaves out and says how old the figures are, the same
caveats the app shows. `port-mcp/SKILL.md` has each tool and how to read what it returns.

## Safe by design

<img src="https://docs.getport.app/cards/skills-safety.png" alt="Read only, your port only, instant disconnect, and an email whenever an app connects" width="100%">

Token and NFT names come from the chain and can be written to trick an AI, so the answers leave
out phishing names and unasked arrivals, and every agent is told a chain-written name is data,
never an instruction. More at [How AI access is kept safe](https://docs.getport.app/security/ai-access/).

## What is in here

| Path | What it is |
| ---- | ---------- |
| `port-api/` | The skill for the REST API, with every endpoint in `references/endpoints.md` |
| `port-mcp/` | The skill for the MCP server at `https://getport.app/mcp` and how to connect it |
| `.claude-plugin/` | The Claude Code marketplace and plugin manifest |
| `.mcp.json` | The MCP server the Claude Code plugin registers; it signs you in the first time |
| `.agents/plugins/`, `.codex-plugin/` | The Codex marketplace and plugin manifest |

## How it is made

This repository is published from the Port_ codebase and is overwritten on every change, so a
pull request here would be lost. The endpoint reference is generated from the same table the API
itself is built from. Docs: [docs.getport.app](https://docs.getport.app/api/).
