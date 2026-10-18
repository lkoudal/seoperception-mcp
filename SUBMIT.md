# Directory submission checklist

Internal checklist for Lars. Safe to keep in the public repo (no secrets), or delete it before the first commit.

`<REPO_URL>` below means `https://github.com/<your-github-username-or-org>/seoperception-mcp`.

Reusable text:

- Name: `SEO Perception`
- One-liner: `Search Console, crawl, and your open SEO to-dos, read-only inside Cursor and Claude.`
- Short description: `SEO Perception stores weekly Search Console facts, the site crawl, and Possibilities (a prioritized fix list per URL). Nine read-only tools let your AI assistant read that workspace and draft the next edit. It never calls Google live and never writes. Connect with OAuth 2.1 or a personal access token. SEO Perception has a free tier; MCP is currently on the Starter plan and above.`
- Endpoint: `https://seoperception.com/mcp` (streamable HTTP)
- Docs: `https://seoperception.com/mcp-integration`
- Registry name: `com.seoperception/mcp`
- Categories: SEO, Marketing, Analytics
- Logo: `assets/logo.png` (512x512), `assets/logo-400.png` (400x400, for Cline)

## 0. Before the first push (fix the TODOs)

- [ ] `glama.json`: replace `TODO-LARS-GITHUB-USERNAME` with your GitHub username.
- [ ] `.cursor-plugin/plugin.json`: replace `TODO-LARS-GITHUB-USERNAME` in `repository` with the real owner (username or org).
- [ ] Check: `grep -rn TODO .` returns nothing.

## 1. Glama (repo listing + ownership)

- URL: https://glama.ai/mcp/servers (Add Server, paste `<REPO_URL>`)
- Paste: `<REPO_URL>`. Glama reads `glama.json` and gives the users in `maintainers` control of the listing.
- Needs your GitHub account: yes (sign in to Glama with the GitHub user named in `glama.json`).
- Note: the hosted connector (`glama.ai/mcp/connectors/com.seoperception/mcp`) is a separate listing claimed via DNS TXT `_glama-claim` or `https://seoperception.com/.well-known/glama.json`. See directories-plan.md. Doing both is fine.

## 2. Cursor Marketplace (official plugin store)

- URL: https://cursor.com/marketplace/publish
- Paste: `<REPO_URL>`. The repo root is the plugin root: `.cursor-plugin/plugin.json` points to `mcp.json` and `assets/logo.png`.
- Needs your GitHub account: the repo must be public under your account or org. Submit with your Cursor account. Manual review by Cursor.
- Test first: copy the repo to `~/.cursor/plugins/local/seo-perception/`, restart Cursor, confirm the server connects and OAuth sign-in works.

## 3. cursor.directory (Auto tab)

- URL: https://cursor.directory/plugins/new (open in a real browser, it has a bot check)
- Paste: `<REPO_URL>` in the Auto tab. If Auto does not pick it up, use the Manual tab with the one-liner, homepage `https://seoperception.com/mcp-integration`, keywords `seo, search console, gsc, crawl, content decay, cannibalization, internal links`, and config `{"mcpServers":{"seo-perception":{"url":"https://seoperception.com/mcp"}}}`.
- Needs your GitHub account: GitHub or Google login.

## 4. Cline MCP Marketplace

- URL: https://github.com/cline/mcp-marketplace/issues/new?template=mcp-server-submission.yml
- Paste:
  - GitHub Repository URL: `<REPO_URL>`
  - Logo Image: drag in `assets/logo-400.png` (400x400 PNG)
  - Tick both testing boxes only after you have actually tested: in Cline, ask "Install the MCP server from `<REPO_URL>`" and confirm it adds the remote entry from `llms-install.md` and signs in.
  - Additional Information: `Hosted remote server (streamable HTTP, OAuth 2.1 with dynamic client registration). Nothing to install locally; llms-install.md tells Cline to add a remote entry only. 9 read-only tools over stored Search Console, crawl and prioritized SEO fix data for the user's own sites. Free tier app; MCP on the Starter plan. Docs: https://seoperception.com/mcp-integration`
- Needs your GitHub account: yes (the issue is opened by you).

## 5. MCP Market

- URL: https://mcpmarket.com/submit (MCP Server tab)
- Paste: `<REPO_URL>` and your email.
- Choice: Free queue (about 4 to 6 weeks) or a paid fast listing (shown as $29 one-time on 18 Oct 2026).
- Needs your GitHub account: no login, just the public repo URL.

## 6. mcp.so

- Free route: open an issue at https://github.com/chatmcp/mcpso/issues/new
  - Title: `Submit Remote MCP Server: SEO Perception`
  - Body: Name, endpoint `https://seoperception.com/mcp` (streamable HTTP), repository `<REPO_URL>`, website `https://seoperception.com/mcp-integration`, the short description above, auth `OAuth 2.1 (browser consent) or a personal access token sent as Authorization: Bearer <token>`, tools `list sites, site overview, search queries, opportunities, page diagnostics, content gaps, content decay, cannibalization, internal link suggestions`, category `SEO / Marketing / Analytics`.
- Paid route: https://mcp.so/submit, Remote Server tab, Repository URL `<REPO_URL>`, Name `SEO Perception` ($39 one-time on 18 Oct 2026).
- Needs your GitHub account: yes for the free issue.

## 7. mcp.directory

- URL: https://mcp.directory/submit
- Paste: `<REPO_URL>`, plus the one-liner if asked.
- Needs your GitHub account: no, only the public repo URL (form may ask for login).

## 8. Docker MCP Registry (remote server PR)

- URL: https://github.com/docker/mcp-registry (fork, then PR)
- Paste: copy `submissions/docker-mcp-registry/seo-perception/` (server.yaml, tools.json, readme.md) to `servers/seo-perception/` in your fork. Or run `task remote-wizard` there (name `seo-perception`, category `analytics`, transport `streamable-http`, URL `https://seoperception.com/mcp`, OAuth yes) and compare.
- PR title: `Add SEO Perception remote MCP server`
- Test (optional, needs Docker Desktop): `task catalog -- seo-perception`, `docker mcp catalog import $PWD/catalogs/seo-perception/catalog.yaml`, `docker mcp server enable seo-perception`, `docker mcp oauth authorize seo-perception`.
- Docker reviews every PR and may ask for test credentials via their form (linked in CONTRIBUTING.md). Use a demo account with a synced site, never your main login.
- Needs your GitHub account: yes (fork + PR).

## 9. Optional: link the repo from the Official MCP Registry

`com.seoperception/mcp` 1.0.0 is already published. If you want the registry entry to show this repo, replace `server.json` with:

```json
{
  "$schema": "https://static.modelcontextprotocol.io/schemas/2025-12-11/server.schema.json",
  "name": "com.seoperception/mcp",
  "title": "SEO Perception",
  "description": "Search Console, crawl and SEO to-dos, read-only inside Cursor and Claude.",
  "version": "1.0.1",
  "websiteUrl": "https://seoperception.com/mcp-integration",
  "repository": { "url": "https://github.com/YOUR-GITHUB-USERNAME/seoperception-mcp", "source": "github" },
  "remotes": [
    { "type": "streamable-http", "url": "https://seoperception.com/mcp" }
  ]
}
```

(the `repository` line is new and `version` is bumped to `1.0.1`), then run `mcp-publisher publish` again with the same domain key. Not required for any directory above.

## Do not

- Do not add any file from the private app repo (no `src/`, no server code, no `.env`, no keys).
- Do not commit `key.pem` or any token.
