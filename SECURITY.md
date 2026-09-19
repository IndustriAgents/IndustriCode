# Security Policy

## What this app has access to

IndustriCode sits between three things that each deserve care:

- **Model provider API keys.** `VITE_*` variables are compiled into the
  frontend bundle by Vite, which means **anyone who can load the page can read
  the key**. That is acceptable for a tool you run on your own machine; it is
  not acceptable for a deployment anyone else can reach. If you host this, move
  the provider calls behind `mcp-backend` and keep the keys server-side.
- **MCP server processes.** `mcp-backend` starts the commands in your MCP
  configuration. A configuration file is therefore a list of commands that will
  be executed on the host — treat one you did not write with the same suspicion
  as a shell script you did not write.
- **Industrial equipment.** The MCP servers this connects to read and, if
  configured to, write to PLCs. A model that can call a write tool will call it.

## Running it safely

- Run it on `localhost`. There is no authentication in front of the UI or the
  backend's WebSocket, so anything that can reach port 3002/3003 can drive your
  MCP servers.
- Keep `.env` out of git. It is already ignored; check before you commit.
- Start against the mock devices that ship with the MCP servers. Connect to
  real equipment only once you know which tools the model has been given.
- Prefer MCP servers configured read-only. Give write access deliberately, to
  one server at a time.

## Reporting a vulnerability

Please report privately, through
[GitHub private vulnerability reporting](https://github.com/IndustriAgents/IndustriCode/security/advisories/new),
or by email to hi@industriagents.com. Please do not open a public issue.

Tell us what an attacker would gain and how to reproduce it. We will
acknowledge within a week.

## Scope

In scope: the frontend, `mcp-backend`, and how the two handle credentials and
MCP server configuration.

Out of scope: vulnerabilities in the model providers, in Ollama, and in MCP
servers maintained elsewhere — including
[IndustriConnect](https://github.com/IndustriAgents/IndustriConnect/security/policy)
and [OPCUA-MCP](https://github.com/IndustriAgents/OPCUA-MCP/security/policy),
which have their own policies.
