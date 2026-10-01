# Installing the SayBriefly MCP server (for AI agents)

SayBriefly is a **remote, hosted** MCP server. Do not clone, build or run anything.

1. Add a remote MCP server named `saybriefly` with:
   - URL: `https://mcp.saybriefly.com/mcp`
   - Transport: Streamable HTTP (`"type": "streamableHttp"` in Cline)
   - No API key, no environment variables, no command or args.

   Cline (`cline_mcp_settings.json`):

   ```json
   {
     "mcpServers": {
       "saybriefly": {
         "type": "streamableHttp",
         "url": "https://mcp.saybriefly.com/mcp"
       }
     }
   }
   ```

2. Authentication is OAuth 2.1. The first request returns 401 with a `WWW-Authenticate` header pointing at the protected resource metadata. The client registers itself (dynamic client registration) and opens the browser so the user signs in to SayBriefly and clicks Allow. Ask the user to complete that sign-in.

3. If the client cannot do OAuth, ask the user for a personal key from SayBriefly (Settings, Integrations, Connect your AI) and send it as the header `Authorization: Bearer sbk_...`. Never guess or invent a key.

4. Verify by calling `tools/list`. Expect 5 read-only tools: `list_meetings`, `get_meeting`, `search_meetings`, `list_todos`, `ask`. Then try `list_todos` to confirm data comes back.

The user needs a SayBriefly account (https://saybriefly.com). Docs: https://saybriefly.com/mcp
