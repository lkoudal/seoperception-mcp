# Installing SEO Perception MCP in Cline

SEO Perception MCP is a **hosted, remote** server. There is nothing to clone, build, or run locally. Do not run `npm install`, `git clone`, or any local command. Only add a remote server entry.

## Server details

- Name: `seo-perception`
- URL: `https://seoperception.com/mcp`
- Transport: Streamable HTTP (`"type": "streamableHttp"`)
- Auth: OAuth 2.1 (browser sign-in, dynamic client registration, PKCE). A personal access token sent as `Authorization: Bearer <token>` also works.
- Ten tools. Nine read stored Search Console, crawl, and Possibilities. `seoperception_update_possibilities` can mark existing to-dos done or ignored, and only when the connection has the `mcp:possibilities` scope.

## Steps

1. Open the Cline MCP settings file (`cline_mcp_settings.json`). In the Cline panel: **MCP Servers > Configure > Configure MCP Servers**. For Cline CLI the file is `~/.cline/data/settings/cline_mcp_settings.json`.
2. Add this entry inside `mcpServers`, keeping any existing servers:

```json
{
  "mcpServers": {
    "seo-perception": {
      "type": "streamableHttp",
      "url": "https://seoperception.com/mcp",
      "disabled": false,
      "autoApprove": []
    }
  }
}
```

3. Save the file. Cline connects and opens the SEO Perception sign-in in the browser. Ask the user to sign in and approve access.
4. Verify by calling `seoperception_list_sites`. It returns the user's sites. Most other tools need a `siteId` from that result.

## If OAuth does not start (token fallback)

Ask the user to sign in at https://seoperception.com, open **Account > MCP**, and create a personal access token (shown once). Never ask for the user's password. Then use:

```json
{
  "mcpServers": {
    "seo-perception": {
      "type": "streamableHttp",
      "url": "https://seoperception.com/mcp",
      "headers": {
        "Authorization": "Bearer PASTE_TOKEN_HERE"
      },
      "disabled": false,
      "autoApprove": []
    }
  }
}
```

## Notes for the agent

- Always set `"type": "streamableHttp"`. Without it Cline uses the legacy SSE transport, which this server does not use.
- 403 `paid_plan_required` means the account is past the trial and has no plan that includes MCP. A 7-day Starter trial includes MCP. After that, Starter, Pro, Business, or Enterprise is required (https://seoperception.com/pricing).
- Empty results usually mean the site has no stored data yet.
- Docs: https://seoperception.com/mcp-integration
