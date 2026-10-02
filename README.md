<p align="center"><img src="logo-400.png" width="96" alt="SayBriefly"></p>

# SayBriefly MCP server

[SayBriefly](https://saybriefly.com) helps freelancers and studios deliver what was agreed and stop scope creep. It turns client meetings and email into tasks and one to-do list, and checks every new request against the locked project brief.

This server lets Claude, ChatGPT, Cursor, Cline or any MCP client read that work: ask what a client agreed to, what changed since the last call, or whether a new request is in the brief.

This is a **hosted (remote) MCP server**. There is nothing to install or run: you connect to a URL and sign in.

```
https://mcp.saybriefly.com/mcp
```

- Transport: Streamable HTTP
- Auth: OAuth 2.1 (sign in with your SayBriefly account; dynamic client registration, PKCE)
- Access: **read-only**. It cannot send, edit or delete anything.
- Docs: https://saybriefly.com/mcp

Listed in the [Claude connector directory](https://claude.ai/directory/saybriefly), the [official MCP Registry](https://registry.modelcontextprotocol.io) (`com.saybriefly/saybriefly`), [Smithery](https://smithery.ai/servers/saybriefly/saybriefly) and [Glama](https://glama.ai/mcp/connectors/com.saybriefly/saybriefly).

## Tools

| Tool | What it does |
|---|---|
| `list_meetings` | List your recorded meetings, newest first, by date range or title |
| `get_meeting` | One meeting in full: summary, decisions, action items; the transcript when you ask for it |
| `search_meetings` | Find where a word or phrase was said, across transcripts and summaries |
| `list_todos` | Your to-dos as the SayBriefly To-Do list shows them: open or done, due before a date, by project |
| `ask` | Ask a plain-language question answered from your meetings, email, tasks and projects |

## Connect

You need a SayBriefly account (Solo, Studio or the 14-day trial). Meetings are recorded with the SayBriefly desktop app for Mac or Windows.

### Claude
Open [SayBriefly in the Claude directory](https://claude.ai/directory/saybriefly) and click **Connect to Claude**. Or add a custom connector with the URL above.

### ChatGPT
Settings, Apps and Connectors, Advanced, turn on Developer mode, Create. Paste the URL, choose OAuth, then sign in and allow.

### Cursor
Settings, MCP, Add new MCP server:

```json
{
  "mcpServers": {
    "saybriefly": { "url": "https://mcp.saybriefly.com/mcp" }
  }
}
```

### Cline
Add a remote server in Cline's MCP settings (`cline_mcp_settings.json`):

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

Cline opens your browser to sign in with SayBriefly the first time you use a tool.

### Claude Code

```bash
claude mcp add --transport http saybriefly https://mcp.saybriefly.com/mcp
```

### Clients without OAuth
Create a personal key in SayBriefly (Settings, Integrations, Connect your AI) and send it as `Authorization: Bearer sbk_...`.

## Example prompts

- "What did we decide in my last client meeting, and what is still open?"
- "Which of my open to-dos are due this week? Group them by project."
- "Search my meetings for 'invoice' and tell me who brought it up."
- "Summarize my meetings from the last two weeks and list every action item assigned to me."

## Privacy

Each connection reads only the signed-in user's own data. Disconnect any time from your AI client, or remove keys in SayBriefly settings. See the [privacy policy](https://saybriefly.com/privacy).

## Support

inbox@saybriefly.com · https://saybriefly.com/support

This repository holds the public connection guide and registry metadata. The server itself is hosted by SayBriefly.
