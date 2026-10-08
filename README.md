# 🧠 MCP Client TypeScript

A robust TypeScript-based tool-calling agent framework that integrates OpenAI's GPT-4o with MCP Tool Servers, featuring real-time Chain-of-Thought reasoning via Server-Sent Events (SSE).

## 🚀 Features

- 🔗 **Tool-Calling via MCP Protocol** - Seamless integration with any MCP-compatible tool server
- 🧠 **ReAct Planning Loop** - Structured reasoning using Plan/Thought/Action/Observation/Final Answer
- 💬 **Streaming with SSE** - Real-time visibility into agent thought processes 
- ⚙️ **Dynamic System Prompting** - Automatic prompt configuration based on available tools
- 🔧 **Easily Extendable** - Simple configuration via `mcp-config.json`
- 🔄 **Session Management** - Maintain context across multiple interactions

## 📦 Project Structure

```
.
├── index.ts                 # Main entrypoint
├── config/
│   └── mcp-config.json      # MCP tool server configurations
├── public/
│   └── index.html           # Basic frontend for testing SSE
├── src/
│   ├── server.ts            # Express application logic
│   ├── routes/
│   │   └── sseRoutes.ts     # Routes for /stream-sse and /clear-session
│   └── services/
│       └── mcpClient.ts     # Core logic (sessions, OpenAI integration, MCP tools)
```

## ⚙️ Setup Instructions

### Prerequisites

- Node.js (v16+)
- npm or yarn
- OpenAI API key

### 1. Install Dependencies

```bash
npm install
```

### 2. Environment Configuration

Create a `.env` file in the root directory:

```env
OPENAI_API_KEY=sk-...
PORT=3000                # Optional, defaults to 3000
```

### 3. Configure Tool Servers

Edit `config/mcp-config.json` to define your MCP tool servers:

```json
{
  "mcpServers": {
    "bsc-mcp": {
      "args": ["./mcp/bsc-tool-server.js"]
    },
    "weathr-mcp": {
      "args": ["./mcp/weather-tool-server.js"]
    }
  }
}
```

Each server should be an MCP-compatible program that communicates via stdin/stdout.

## 🛠️ Build & Run

### Production Build

```bash
npm run build
```

This command:
- Compiles TypeScript to JavaScript
- Copies `config/` and `public/` directories to the `build/` folder

### Start the Server

```bash
npm start
```

This runs the compiled application from the `build/` directory.

### Development Mode

```bash
npm run dev
```

Uses `ts-node-dev` to automatically rebuild and restart on file changes.

## 📝 Usage Guide

### Testing Locally

Visit `http://localhost:3000` in your browser to access the basic test interface.

Enter a prompt in the text area and observe the agent's step-by-step reasoning in real-time.

### API Endpoints

| Endpoint | Method | Parameters | Description |
|----------|--------|------------|-------------|
| `/stream-sse` | GET | `prompt`, `session_id` (optional) | Streams agent responses in SSE format |
| `/clear-session` | GET | `session_id` | Clears a specific session's memory |

### Curl Examples

```bash
# Stream a response
curl -N "http://localhost:3000/stream-sse?prompt=What's+the+weather+in+Goa&session_id=test123"

# Clear a session
curl "http://localhost:3000/clear-session?session_id=test123"
```

## 🧠 Example Prompts

### Weather Information (weathr-mcp)

```
What's the weather like in Goa today?
```

### Blockchain Operations (bsc-mcp)

```
Create a BEP-20 token called "Sanjal" with 18 decimals and a total supply of 1 million.
```

```
Check security of token address 0xabc123...
```

```
Transfer 1 BNB from 0xA to 0xB.
```

## 🧠 Agent Response Format

The agent follows a ReAct-style reasoning format:

```
Plan: First, I will get the current weather in Goa.
Thought: To answer the question, I need to use the weatherReport tool.
Action: weatherReport({ location: "Goa" })
Observation: The weather in Goa is sunny with 32°C.
Final Answer: It's sunny and 32°C in Goa right now.
```

## 🔧 Customization Options

### Adding New Tool Servers

1. Create an MCP-compatible tool server script
2. Add the server to `config/mcp-config.json`
3. Restart the application

### Configuration Parameters

Edit `src/services/mcpClient.ts` to modify:
- Maximum history length
- OpenAI model selection
- System prompt templates
- Temperature settings

## 🐳 Dockerization (Optional)

### Dockerfile

```dockerfile
FROM node:18-alpine

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .
RUN npm run build

EXPOSE 3000

CMD ["npm", "start"]
```

### Building and Running

```bash
docker build -t mcp-client .
docker run -p 3000:3000 -e OPENAI_API_KEY=sk-... mcp-client
```

## 🚀 Future Enhancements

- [ ] Auto-detection of tool capabilities
- [ ] OpenAPI specification generation for available tools
- [ ] Integration with LangChain and Hugging Face
- [ ] Advanced session management with database storage
- [ ] Authentication and multi-user support
- [ ] Comprehensive logging and analytics

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

---

Built with ❤️ using TypeScript, Express, and OpenAI
## Portfolio

More about this project: https://theani.me/work/mcp-client
