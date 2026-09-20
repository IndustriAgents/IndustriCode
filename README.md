# IndustriCode

[![License: MIT](https://img.shields.io/github/license/IndustriAgents/IndustriCode)](LICENSE)
[![MCP](https://img.shields.io/badge/Model_Context_Protocol-client-0b7285)](https://modelcontextprotocol.io)

A chat interface for working with language models that can reach industrial
systems. It connects to MCP servers — the ones in
[IndustriConnect](https://github.com/IndustriAgents/IndustriConnect) and
[OPCUA-MCP](https://github.com/IndustriAgents/OPCUA-MCP), or any other — and
puts them in front of a cloud or local model, so an agent can call a tool that
reads a PLC in the same conversation you are having with it.

Built with React, TypeScript, Vite and Tailwind CSS.

> **This talks to industrial equipment.** Point it at the mock devices that
> ship with the MCP servers before you point it at anything real, and read
> [SECURITY.md](SECURITY.md) first.

## Features

- 🎨 **Beautiful UI** – Modern interface inspired by Claude Code with clean design  
- 🌓 **Dark/Light Mode** – Seamless theme switching with system preference detection  
- 🔌 **MCP Server Integration** – Connect and manage Model Context Protocol servers (MQTT, OPC UA, etc.)
- ☁️ **Cloud LLM Support** – Talk to OpenAI GPT-5 models, Google Gemini, and Anthropic Claude  
- 💻 **Local LLM Support (Ollama)** – Chat with local models running via Ollama  
- 💬 **Interactive Chat** – Streaming‑style conversational UI with copy‑to‑clipboard  
- 📝 **Session Management** – Simple session list to keep track of conversations  
- 💾 **Local Storage** – Remembers chat backend and LLM configuration between visits

## Getting Started

### Prerequisites
- Node.js (v18 or higher)
- npm or yarn
- Python (for running MCP servers like MQTT/OPC UA)
- `uv` (Python package manager)

### Installation

1. Install dependencies (including backend):
   ```bash
   npm install
   cd mcp-backend && npm install && cd ..
   ```

### Configure API keys

Copy the example env file and drop your provider keys in. The app will read these automatically, so you won't be prompted for a key in the UI.

```bash
cp .env.example .env
# then set VITE_OPENAI_API_KEY / VITE_GEMINI_API_KEY / VITE_ANTHROPIC_API_KEY
```

### Running the Application

Start both the frontend UI and the backend service with a single command:

```bash
npm run dev
```

This will start:
- Frontend UI at http://localhost:5173 (or similar)
- Backend WebSocket server at ws://localhost:3003
- Backend HTTP server at http://localhost:3002

## How the pieces fit

```text
you ─► IndustriCode (browser)
        │  WebSocket :3003 / HTTP :3002
        ▼
     mcp-backend  ──stdio─►  MCP server (Modbus, OPC UA, MQTT, …)  ──fieldbus─►  device
        │
        └── cloud model (OpenAI / Gemini / Anthropic) or local Ollama
```

The browser never speaks to an MCP server directly. `mcp-backend` owns the
server processes, because MCP runs over stdio, and relays tool calls and status
over a WebSocket.

## Usage

1. **Configure MCP Servers** (New!)
   - Click "Configure Servers" in the MCP Servers section of the sidebar
   - Option 1: Use the form to add servers manually
   - Option 2: Import a Cursor-style JSON configuration file
   - Option 3: Use JSON editor mode to paste configuration directly
   - Example configuration available in `mcp-config-example.json`

2. **Connect to MCP Servers**
   - Start your MCP server processes externally (e.g., run the MQTT or OPC UA servers)
   - In the sidebar, click "Connect" next to each configured server
   - View available tools by expanding the server entry
   - Connected servers will show a green indicator

3. **Choose a Chat Backend**
   - In the top bar of the chat panel, select:
     - `Cloud LLM (OpenAI GPT-5 / Gemini / Claude)` or  
     - `Local Ollama`

4. **Configure Cloud LLMs**
   - Select a provider (OpenAI, Gemini, Anthropic)
   - Pick a model from the dropdown
   - Provide an API key either:
     - via `.env` file (`VITE_OPENAI_API_KEY`, `VITE_GEMINI_API_KEY`, `VITE_ANTHROPIC_API_KEY`), or  
     - directly in the UI when prompted

5. **Configure Ollama**
   - Ensure Ollama is running locally (default: `http://localhost:11434`)
   - Select or refresh the model list in the header

6. **Start Chatting**
   - Type your message in the input box
   - Press `Enter` to send, `Shift+Enter` for a new line
   - Click the copy icon on assistant messages to copy responses

7. **Manage Sessions**
   - Sessions are created automatically when you start chatting
   - Use the sidebar to switch between sessions

## Project Structure

```text
IndustriCode/
├── mcp-backend/           # Node service that owns the MCP server processes
├── MCPs/                  # MQTT and OPC UA servers, for trying it out locally
├── src/
│   ├── components/        # React components
│   │   ├── Sidebar.tsx    # Left sidebar with sessions and theme toggle
│   │   ├── ChatPanel.tsx  # Right panel for chat interface
│   │   └── ThemeProvider.tsx   # Theme context provider
│   ├── types.ts           # TypeScript type definitions
│   ├── utils/             # Utility functions
│   │   ├── theme.ts       # Theme management
│   │   ├── storage.ts     # LocalStorage helpers
│   │   └── llm.ts         # Cloud/Ollama LLM helpers
│   ├── App.tsx            # Main application component
│   └── main.tsx           # Entry point
├── index.html
├── package.json
└── vite.config.ts
```

## Future Enhancements

- [ ] Streaming responses
- [ ] Per-session message history persistence
- [ ] Command/Prompt templates
- [ ] Multi-tab support for multiple sessions

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for how to run the frontend and backend
together and what to check before opening a pull request, and the
[Code of Conduct](CODE_OF_CONDUCT.md) for how we work together.

## Security

API keys, MCP server processes and industrial equipment all meet in this app.
[SECURITY.md](SECURITY.md) covers what that means, and how to report a
vulnerability privately.

## License

Released under the [MIT License](LICENSE).
