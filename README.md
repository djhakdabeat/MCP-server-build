# Simple MCP Server for Claude Desktop


This is a minimal MCP server example that exposes a simple tool Claude Desktop can call.


## Requirements
- Node.js 18+
- Claude Desktop (Developer Mode enabled)


## Installation
```bash
npm install

## Run the MCP server
-node server.js

## Add to Claude Desktop
Edit (or create) the config file:

macOS: ~/Library/Application Support/Claude/claude_desktop_config.json
Windows: %APPDATA%/Claude/claude_desktop_config.json
Add:
{
"mcpServers": {
"my-mcp-server": {
"command": "node",
"args": ["/ABSOLUTE/PATH/TO/my-mcp-server/server.js"],
"cwd": "/ABSOLUTE/PATH/TO/my-mcp-server"
}
}
}

## Restart Claude Desktop.


---


## package.json
```json
{
"name": "my-mcp-server",
"version": "1.0.0",
"description": "Minimal Claude MCP server example",
"main": "server.js",
"type": "module",
"dependencies": {
"@modelcontextprotocol/sdk": "^1.0.0"
}
}

## mcp.config.json (Optional)
import { Server } from "@modelcontextprotocol/sdk";
import { StdioServerTransport } from "@modelcontextprotocol/sdk/transports/stdio.js";


// Create server
const server = new Server({ name: "simple-mcp", version: "1.0.0" });


// Register a simple tool
server.tool("hello_world", {
description: "Returns Hello + name",
inputSchema: {
type: "object",
properties: {
name: { type: "string" }
},
required: ["name"]
}
}, async ({ name }) => {
return { content: [{ type: "text", text: `Hello, ${name}!` }] };
});


// Start the server over stdio
await server.connect(new StdioServerTransport());
console.log("MCP server running...");

## Next Steps
Next Steps

Copy these files into a folder named my-mcp-server/

Run npm install

Register it in Claude Desktop config

Restart Claude Desktop

You now have a working MCP server Claude can call!
