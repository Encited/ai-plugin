# Encited AI plugin

Official Encited plugin for AI clients. Ask about your own site from your AI coding tool and get answers from live data: is Googlebot visiting, why a page is not indexed, which queries earn clicks, what ChatGPT says about your brand, and what to write next. Then run the fix from the same chat: start an audit, launch a content workflow, or build a client report.

## Installation

Every client signs in the same way: on first use of an Encited tool, the client opens Encited sign-in in the browser. Sign in with your Encited account. Do not paste an API key or token into chat.

### Claude Code

1. Install the plugin:
    ```bash
    claude plugin marketplace add Encited/ai-plugin
    claude plugin install encited@encited
    ```

    Or add the server alone, without the skill:
    ```bash
    claude mcp add --transport http encited https://encited.com/api/mcp
    ```

2. Authenticate via OAuth:
    ```bash
    claude
    # Then run /mcp, select encited, and press Enter
    /mcp
    ```
    Then follow the browser prompts to log into Encited.

### Claude (web and desktop)

Add Encited from the [Claude connector directory](https://claude.ai/directory/encited), then sign in when the browser window opens.

### Cursor

Add manually in Cursor Settings > Plugins, or add `https://encited.com/api/mcp` to `~/.cursor/mcp.json` under `mcpServers`.

### Codex

1. Add the marketplace:
    ```bash
    codex plugin marketplace add Encited/ai-plugin
    ```

2. Install the plugin from inside Codex:
    ```
    codex
    # Then run /plugins, select Encited, and install
    /plugins
    ```

### Grok Build

```bash
grok plugin install Encited/ai-plugin --trust
```

### Any other MCP client

Add `https://encited.com/api/mcp` as an HTTP (streamable) server. Per-client steps for ChatGPT, Lovable, and others are at [encited.com/seo-mcp](https://encited.com/seo-mcp).

## How to develop

```bash
git clone https://github.com/Encited/ai-plugin
claude --plugin-dir ./ai-plugin
```

Then run `/mcp` and follow the browser prompts to log into Encited.

## Features

The plugin provides 55 Encited tools across these categories. Every tool takes a domain from your account; the agent calls `list_domains` first.

- **Crawl analytics**: which crawlers and AI bots visit, how often, and what they got back
- **Index status**: sitemap URLs with Google index status, and which indexed pages to recrawl so a new page gets discovered
- **Search Console**: queries, per-page clicks and impressions, period totals, and live URL inspection
- **Technical audit**: run a site audit, then read the aggregate issues or one page's full record
- **Pre-rendering**: what crawlers see on a page, plus middleware setup and verification for edge hosts
- **Keyword research**: keyword ideas, competitor gaps, and coverage checks against what you already rank for
- **AI visibility**: how ChatGPT, Perplexity, and other engines mention your brand, competitors' share, cited sources, and the searches behind each answer
- **Content workflows**: Search Console audits, SERP checks, competitor discovery, content plans, article drafts, backlink research, and Google Maps grid scans
- **Domains and reports**: onboard a site, change its settings, and build a shareable client report

Tools that a plan does not include return `plan_upgrade_required` with the plan needed. Keyword searches and workflows spend credits from the account.

### Bundled skill

The plugin ships one skill, `encited`, that routes SEO, content, ranking, and AI-search questions to the right tool and explains how to run a workflow end to end. It activates when its description matches your request, so you do not need to invoke it by name.

## Example usage

```
> Is Googlebot crawling my site?
> Did ClaudeBot read my pricing page this week?

> Why isn't /blog/new-post indexed?
> Which pages should Google recrawl so the new page gets found?

> What queries do we rank for?
> Which pages get impressions but no clicks?

> Audit my site and give me a prioritized fix list
> What does the crawler actually see on /pricing?

> Find keywords a competitor ranks for that we don't
> Do we already rank for these keywords?

> How does ChatGPT describe my brand?
> Which sites do AI answers cite in my space?

> Build a content plan from our AI visibility gaps
> Write the monthly SEO report for this client
```

## Authentication and network

The plugin connects only to `https://encited.com`. Authentication is OAuth 2.1 with PKCE and dynamic client registration.

Endpoints:

- `https://encited.com/api/mcp`: hosted MCP (streamable HTTP)
- `https://encited.com/authorize`, `/oauth/token`, `/oauth/register`: OAuth 2.1 + DCR
- `https://encited.com/.well-known/oauth-authorization-server` and `/.well-known/oauth-protected-resource`: discovery

Credentials: an Encited account. The access token is sent as `Authorization: Bearer` on `/api/mcp`. No API key is stored in the plugin. Tools are scoped to the domains the signed-in user can access, and a connection approved read-only exposes read tools only.

The plugin runs no local code: no hooks, no commands, no scripts.

## Documentation

- [Encited MCP](https://encited.com/seo-mcp)
- [Encited](https://encited.com)
- Support: team@encited.com

## License

The files in this repository are licensed under the [MIT License](LICENSE). Use of the hosted MCP is governed by the [Encited terms](https://encited.com/terms). The Encited name and logo are trademarks of Encited and are not covered by the MIT grant.
