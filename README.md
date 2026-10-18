<p align="center">
  <img src="assets/logo.png" alt="SEO Perception logo" width="120" height="120">
</p>

<h1 align="center">SEO Perception MCP</h1>

<p align="center">
  Search Console, crawl, and your open SEO to-dos, read-only inside Cursor, Claude, VS Code and other MCP clients.
</p>

<p align="center">
  <a href="https://seoperception.com/mcp-integration">Docs</a> ·
  <a href="https://seoperception.com">Website</a> ·
  <a href="https://registry.modelcontextprotocol.io/v0.1/servers?search=com.seoperception">MCP Registry</a>
</p>

---

This repository lists the **hosted** SEO Perception MCP server in MCP directories. It contains documentation and client configuration only. There is nothing to install or run locally: the server runs at `https://seoperception.com/mcp`.

[SEO Perception](https://seoperception.com) stores weekly Google Search Console facts, a crawl of your site, and Possibilities (a prioritized fix list per URL). The MCP server lets your AI assistant read that same workspace and draft the next edit on a page, based on stored evidence instead of guesses.

## At a glance

| | |
|---|---|
| Endpoint | `https://seoperception.com/mcp` |
| Transport | Streamable HTTP (plain JSON responses) |
| Auth | OAuth 2.1 with dynamic client registration and PKCE, or a personal access token as `Authorization: Bearer <token>` |
| Scope | `mcp:read` |
| Tools | 9, all read-only |
| MCP Registry name | `com.seoperception/mcp` |
| Server card | [`/.well-known/mcp/server-card.json`](https://seoperception.com/.well-known/mcp/server-card.json) |

## Plan

SEO Perception has a free tier. MCP access is currently included on the Starter plan and above. See [pricing](https://seoperception.com/pricing) for current details.

## Tools

All tools are read-only. They read data SEO Perception has already collected and never call Google live.

| Tool | Title | What it does |
|---|---|---|
| `seoperception_list_sites` | List sites | List owned websites with stored GSC and crawl freshness. |
| `seoperception_get_site_overview` | Site overview | Summarize stored search performance, leading pages, queries, and rank momentum. |
| `seoperception_search_queries` | Search queries | Search and filter stored query evidence by content match, position band, impressions, landing page, CTR, and intent. |
| `seoperception_list_opportunities` | SEO opportunities | List bounded, evidence-backed crawl and Search Console possibilities. |
| `seoperception_get_page_diagnostics` | Page diagnostics | Combine one page's crawl metadata, stored GSC trends, top queries, and possibilities. |
| `seoperception_find_content_gaps` | Content and snippet gaps | Find stored low-CTR, missing-query, title/snippet, question, and refresh opportunities. |
| `seoperception_find_content_decay` | Content decay | Detect sustained stored click and position declines from open refresh-candidate rows. |
| `seoperception_find_cannibalization` | Keyword cannibalization | Find stored query conflicts and the deterministic preferred page. |
| `seoperception_suggest_internal_links` | Internal-link suggestions | Return stored destination and anchor opportunities without crawling or probing URLs. |

Most tools take a `siteId`. Ask the assistant to list your sites first and it will pick it up.

## Setup

Clients that support OAuth open a browser sign-in the first time they connect. No token needed.

If your client does not support OAuth, sign in to SEO Perception, open **Account > MCP**, create a personal access token (shown once), and store it as the environment variable `SEOPERCEPTION_MCP_TOKEN`. Then use the "token" variant of the config.

### Cursor

Install the **SEO Perception** plugin from the Cursor Marketplace (once listed), or add this to `~/.cursor/mcp.json` (all projects) or `.cursor/mcp.json` (one project):

```json
{
  "mcpServers": {
    "seo-perception": {
      "url": "https://seoperception.com/mcp"
    }
  }
}
```

Cursor shows a sign-in prompt for the server. Token variant:

```json
{
  "mcpServers": {
    "seo-perception": {
      "url": "https://seoperception.com/mcp",
      "headers": {
        "Authorization": "Bearer ${env:SEOPERCEPTION_MCP_TOKEN}"
      }
    }
  }
}
```

### Claude.ai and Claude Desktop (connectors)

1. Open **Settings > Connectors**.
2. Choose **Add custom connector**.
3. Name: `SEO Perception`. URL: `https://seoperception.com/mcp`.
4. Click **Add**, then **Connect** and approve the SEO Perception sign-in.

Custom connector availability depends on your Claude plan.

### Claude Code

```bash
claude mcp add --transport http seo-perception https://seoperception.com/mcp
```

Then run `/mcp` inside Claude Code and choose **Authenticate**.

### VS Code (GitHub Copilot agent mode)

Add to `.vscode/mcp.json` in your project, or run **MCP: Add Server** from the Command Palette and pick HTTP:

```json
{
  "servers": {
    "seo-perception": {
      "type": "http",
      "url": "https://seoperception.com/mcp"
    }
  }
}
```

VS Code starts the OAuth sign-in when the server first connects.

### Windsurf (Devin Desktop)

Add to `mcp_config.json` (`~/.config/devin/mcp_config.json` on current builds, `~/.codeium/windsurf/mcp_config.json` on older Windsurf builds):

```json
{
  "mcpServers": {
    "seo-perception": {
      "url": "https://seoperception.com/mcp"
    }
  }
}
```

Sign in when the server shows **Needs auth**, or use the token variant with a `headers` block as in the Cursor example.

### Cline

In the Cline panel open **MCP Servers > Remote Servers**, enter the name `seo-perception`, URL `https://seoperception.com/mcp`, transport **Streamable HTTP**, then **Add Server**. Or edit `cline_mcp_settings.json`:

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

Set `"type": "streamableHttp"` explicitly; without it Cline falls back to the legacy SSE transport. See [llms-install.md](llms-install.md) for step-by-step instructions an agent can follow.

### Lovable

1. Open **Connectors**, click **+**, choose **MCP server**.
2. Server name: `SEO Perception`. URL: `https://seoperception.com/mcp`.
3. Keep **OAuth**, click **Add & authorize**, and approve the sign-in.

### Any other MCP client

```
URL:            https://seoperception.com/mcp
Transport:      Streamable HTTP
Authorization:  OAuth 2.1 (discovered via WWW-Authenticate), or Bearer <your MCP token>
```

## Example prompts

- "List my SEO Perception sites and tell me which data is freshest."
- "Summarize the strongest gains and losses for my site using stored data."
- "Show the highest-priority open SEO opportunities and explain the evidence."
- "Use SEO Perception to find the biggest losing keywords on /pricing and suggest concrete content changes to recover them."
- "Find non-branded queries in positions 5 to 20 with at least 50 impressions that are missing from their landing page."
- "Find keyword cannibalization and recommend which page should be preferred."
- "Find the biggest content and snippet gaps, then suggest rewrites that match the losing keywords."
- "Suggest internal links and natural anchor text for pages with ranking potential."
- "Which pages show meaningful content decay?"

## Security and data boundary

- **Read-only.** All 9 tools only read. The server never writes to your site, Search Console or SEO Perception data, and never triggers recrawls.
- **No live Google calls.** Tools read the weekly Search Console data, crawl and prioritized fix list SEO Perception has already stored. Collection happens in the product's own sync, not inside a tool call.
- **Scoped access.** OAuth tokens use the `mcp:read` scope and only see sites in your own account.
- **Tokens, not passwords.** Never put your login password or session cookie in an MCP config. Create and revoke personal access tokens under **Account > MCP**.
- **Your client's model provider.** Your MCP client may send returned results to its model provider. Treat them as sensitive site analytics.

## Troubleshooting

- **401**: sign in again, or create a new token under Account > MCP and update `SEOPERCEPTION_MCP_TOKEN`.
- **403 `paid_plan_required`**: MCP needs a plan that includes it (see Plan above).
- **429**: wait for the retry time in the response.
- **Empty lists**: usually no stored data yet for that site.

## Links

- Docs and setup: https://seoperception.com/mcp-integration
- Website: https://seoperception.com
- Pricing: https://seoperception.com/pricing
- Privacy: https://seoperception.com/privacy
- Support: https://seoperception.com/contact
- Server card: https://seoperception.com/.well-known/mcp/server-card.json
- Official MCP Registry: `com.seoperception/mcp`

## About this repository

This repo holds only listing metadata (README, `server.json`, `glama.json`, `llms-install.md`, Cursor plugin manifest, logo). The SEO Perception application and MCP server source code are not open source and are not part of this repository.

Made by [Larsik Corp](https://seoperception.com) (Lars Koudal). Documentation and configuration files are released under the [MIT License](LICENSE). "SEO Perception" and the logo are trademarks of Larsik Corp. Google and Search Console are trademarks of Google LLC; SEO Perception is not affiliated with or endorsed by Google.
