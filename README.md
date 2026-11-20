# Whova API and MCP server for ChatGPT and Claude

Whova has no public API documentation or official MCP server according to the live Cotlet.dev integration page (checked 24 October 2026). Cotlet.dev connects ChatGPT, Claude and other AI agents to your own Whova organizer account through a hosted MCP server with 11 workflow tools. Announcements, speaker emails and attendee additions ask for approval by default.

## What you can ask

> List the Whova events I can manage.

> Read the agenda and draft a revised schedule after a speaker change.

> List new registrations since yesterday and include every page of results.

> Show attendees who have not checked in.

> Draft an announcement about the room change and ask me before sending.

> Read the Sponsors and exhibitors section of the released post-event report.

## Connect in 2 minutes

Get your API key at https://cotlet.dev/app and export it as `COTLET_API_KEY`.

### ChatGPT

Settings > Apps & Connectors > Create. Use `https://cotlet.dev/mcp/v1/connect` with OAuth.

### Claude

Settings > Connectors > Add custom connector. Use `https://cotlet.dev/mcp/v1/connect`.

### Claude Code

```bash
claude mcp add --transport http cotlet https://cotlet.dev/mcp/v1 --header "Authorization: Bearer $COTLET_API_KEY"
```

### Cursor

Copy `.cursor/mcp.json` from this repo into your project:

```json
{
  "mcpServers": {
    "cotlet": {
      "url": "https://cotlet.dev/mcp/v1",
      "headers": {
        "Authorization": "Bearer ${COTLET_API_KEY}"
      }
    }
  }
}
```

## Run it locally (stdio)

This repo is a real MCP server. It speaks MCP over stdio, lists its Whova tools without any key, and forwards every tool call to the hosted Cotlet.dev server. The hosted URL above stays the recommended way. Use this when your client only starts local servers.

Clone this repository and run `python3 whova_agent.py serve` with `COTLET_API_KEY` set.

Or with Docker:

```bash
docker build -t whova-mcp .
docker run -i --rm -e COTLET_API_KEY whova-mcp
```

The local server exposes seven browser helper tools, distinct from the 11 hosted workflow tools described on the live page.

Local tools: `whova_guide`, `whova_status`, `whova_open`, `whova_read`, `whova_click`, `whova_type`, `whova_close`. Without `COTLET_API_KEY` a tool call answers: "Set COTLET_API_KEY. Get a key at https://cotlet.dev/app".

## Command line

Python 3.10+, nothing else to install. Run from this repo:

```bash
export COTLET_API_KEY="your-key"
python3 whova_agent.py connect   # ready config for Claude Code, Cursor, ChatGPT
python3 whova_agent.py status    # is Whova ready in your Cotlet.dev account
python3 whova_agent.py guide     # Whova screens and sign-in link
python3 whova_agent.py read      # open Whova, read the page, close the browser
python3 whova_agent.py tools     # list Cotlet.dev tools
python3 whova_agent.py serve     # start the stdio MCP server (same as no argument)
```

## Questions from Whova users

**Does Whova have an official API or MCP server?**

The live Cotlet.dev page, checked 24 October 2026, reports no public API documentation or official MCP server. Cotlet.dev provides an independent hosted connection to your own organizer account.

**Which account do I need?**

Use your own Whova organizer account with permission to manage the event. Sign in through its Cotlet Cloud Browser and enable Whova for agents.

**Which changes ask for approval?**

Announcements, speaker emails and attendee additions ask for approval by default. Agenda edits run directly. Approval settings can be changed per tool.

**Can an agent request my post-event report?**

Request it under Tools > Report in Whova first. Once Whova releases it, Cotlet.dev can read its sections.

**Is this an official Whova integration?**

No. Cotlet.dev is independent and is not affiliated with Whova.

## Live integration

[Whova tools and connection guide](https://cotlet.dev/integrations/whova-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=whova)

---

More: https://cotlet.dev/integrations/whova-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=whova

Start free: https://cotlet.dev/signup?app=whova&utm_source=github&utm_medium=readme&utm_campaign=whova
