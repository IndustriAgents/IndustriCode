# Contributing to IndustriCode

Thanks for taking an interest. IndustriCode is a React frontend and a small
Node backend: the frontend is the chat UI, and `mcp-backend` owns the MCP
server processes, because MCP runs over stdio and a browser cannot speak that.

## Getting set up

```bash
npm install
cd mcp-backend && npm install && cd ..
cp .env.example .env     # add whichever provider keys you have
npm run dev              # frontend :5173, backend HTTP :3002 and WS :3003
```

`npm run dev` starts both halves. If you are only touching the UI,
`npm run dev:frontend` is enough — but tool calling will not work without the
backend.

You do not need real equipment. `MCPs/` has MQTT and OPC UA servers with mock
devices, which is what you should develop against.

## Before you open a pull request

```bash
npm run lint      # eslint, --max-warnings 0
npm run build     # tsc && vite build — the type check is the useful half
```

Both must pass. There is no test suite yet; if you are adding logic that is
worth testing, adding the first one is a welcome contribution in itself.

Then use the thing: start the backend, connect an MCP server from `MCPs/`, and
have an actual conversation that calls a tool. Most of what breaks here breaks
in the wiring between the three processes, not in a unit.

## House rules

- **Never commit a key.** `.env` is ignored — keep it that way, and do not
  paste a key into an issue or a screenshot.
- **`VITE_*` is public.** Anything prefixed `VITE_` is compiled into the bundle
  and readable by anyone who loads the page. If you are adding a secret that
  must stay secret, it belongs in `mcp-backend`, not in the frontend.
- **Assume the MCP server can move a machine.** Anything that makes it easier
  to fire a tool — a shortcut, a retry, an auto-approve — needs to be thought
  about in those terms. Say so in the pull request if your change touches that
  path.
- Keep the backend's WebSocket messages typed. `src/types.ts` is the contract
  between the two halves; change it in one place.

## Pull requests

Say what you changed, which half it touches, and how you exercised it — which
MCP server, which tool, what came back. Screenshots help for UI changes.

## Code of Conduct

By taking part you agree to the [Code of Conduct](CODE_OF_CONDUCT.md).
