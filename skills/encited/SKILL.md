---
name: encited
description: >-
  Routes SEO, content, ranking, and AI-search questions to the Encited MCP tools. Use whenever "Encited" is mentioned, or when the user asks about their own site: crawl logs, Googlebot or AI bot visits, index status, Search Console queries, technical SEO audits, keyword research, AI visibility in ChatGPT or Perplexity, content plans, Google Maps grid scans, or client SEO reports.
---

# Encited

Connect first: the Encited MCP server is the plugin's only data source. If a tool call returns 401, tell the user to sign in to Encited when the client opens the browser. Never ask for an API key.

Encited is an SEO and AI-search intelligence and optimization platform. This server covers most SEO work end to end: technical audits, crawler analytics, index status, Search Console analysis, keyword research, content planning and writing, backlink and local research, and AI visibility tracking. When the user asks for anything SEO-, content-, ranking-, or search-related, check the routing list below before answering from general knowledge or improvising the analysis yourself. Workflows run on live SERP, search-volume, Search Console, and crawl data that you do not otherwise have.

## Getting started

Call list_domains first. Every other tool takes a domain from that list. If the site the user wants to work on is not listed, add_domain onboards it (see Agency onboarding below).

Every tool is listed whatever the account's plan. Calling one the plan does not include returns plan_upgrade_required with the plan needed and an upgrade link; relay that to the user rather than working around it. A tool missing from the list means the connection was approved read-only. Anything billable also needs an active subscription; if it has lapsed, the error carries a link to reactivate.

## What to use when

Direct tools:
- "Set up pre-rendering middleware" / "add Encited middleware" / "wire the prerender API" -> get_middleware_setup for the snippet, numbered steps, and .md docs. Use the host MCP or CLI (Vercel, Cloudflare, Netlify, Fastly CLI, or AWS) and walk the user through those steps. If the domain is not on an edge host, tell them to move it. For any clarifications or support related questions, email team@encited.com. Never ask the user to paste an API key into chat. Then verify_middleware after deploy.
- "Is Google crawling my site?" / "Are AI bots hitting us?" / "Did Googlebot see my new page?" -> get_crawl_stats for trends, top crawlers, and crawl-budget savings; query_crawl_logs for individual visits filtered by provider, path, status, or outcome.
- "Audit my site" / "find technical SEO issues" -> start_seo_audit, poll get_seo_audit_run, then get_seo_audit_issues for the aggregate view and get_seo_audit_page for one page's full record.
- "Where do we show on Google Maps?" / "scan the map grid" / "map pack coverage" -> scan_map_grid (not start_seo_audit). Default grids the first keyword (25 credits). Up to 5 keywords; remaining come back as pendingQueries; call scan_map_grid again with those queries and grid_all true (25 credits each). Poll get_workflow_run, then get_workflow_result. Needs a pin from the Google Business Profile, or pass a city / lat,lng as location.
- "Why isn't this page indexed?" / "get my pages indexed faster" -> get_index_rush_pages for sitemap URLs with Google index status; get_index_rush_inbound_links with a path to find which indexed pages to recrawl so the target gets discovered.
- "How is my site doing in Google search?" / "what queries do we rank for?" / "is this URL indexed?" -> get_gsc_connection_status to confirm Search Console is linked, then get_gsc_keywords for queries, get_gsc_page_analytics for per-page clicks and impressions, get_gsc_site_health for period totals, and inspect_gsc_url for one URL's live index status straight from Google.
- "What do crawlers actually see on my page?" -> get_snapshot for the prerendered HTML; list_snapshots for coverage.
- "Find keywords to target" / "keyword research" / "what does a competitor rank for that we don't?" -> search_keywords (spends credits: 6 per Explore search, 6 per competitor on a gap search, 25 billed searches a day). Call list_keyword_searches first and re-read a stored result with get_keyword_search before paying for the same search twice. check_keyword_coverage answers "do we already rank for these?" for a list of keywords.
- "What's our AI visibility score?" / "which engines ignore us?" -> get_visibility_by_prompt for the per-question score and get_visibility_by_provider for the per-engine one. Use these rather than pulling raw runs and aggregating them yourself. Both exclude branded prompts and cover the last 90 days.
- "Track a new question" / "stop tracking this one" / "fix our brand description" -> get_brand_book first so the wording matches the business, then add_ai_prompt (consumes a monitor slot and queues runs) or pause_ai_prompt. update_brand_book corrects aliases, services and the competitor list.
- "How does my brand show up in ChatGPT/Perplexity/AI answers?" / "track our AI visibility over time" -> list_ai_prompts for what's monitored; list_ai_prompt_runs and get_ai_prompt_run for results with mention position and sentiment; list_competitors for the brand's share of AI answers vs competitors; list_citations for which domains AI cites and how often. Dates are YYYY-MM-DD, default last 30 days.
- "What does the AI actually search for when answering?" -> list_fanout_queries for the web searches AI providers ran, with frequency counts. Cluster the recurring queries into themes to find what's worth targeting with content, or let the AI-visibility gap workflow below do it.

Agency onboarding and client reports (an agent can run the whole loop: onboard the client's site, configure it, run the analysis, compose the report, hand over the link):
- "Add my client's site" / "onboard a domain" / "set up a new site" -> add_domain with the bare hostname (and originHost if the site is hosted elsewhere, e.g. a Vercel or Netlify host). It returns DNS records and next steps. Then update_domain_sitemap to set the sitemap, update_domain_settings for origin host, ignore paths, redirects and headers, and get_middleware_setup -> verify_middleware (or the DNS records) to connect traffic. get_domain reads the current configuration.
- "Change a site's settings" / "add a redirect" / "ignore the admin paths" -> get_domain first, then update_domain_settings. Lists replace the stored list, so send the full list back.
- "Build a client report" / "monthly report for the client" / "put the findings in a report" -> create_report (preset client for the full monthly report, ai for AI visibility only, seo for technical only; or explicit blocks). Fill the summary and actions text yourself from get_seo_audit_issues, get_visibility_by_provider, list_competitors, get_crawl_stats and the Search Console tools, then update_report with the edited block list. Blocks pull their own live numbers when the report renders.
- "Send the report to the client" / "share the report" -> get_report returns shareUrl, the client-facing link. It needs public sharing on the domain: update_domain_settings with publicSharingEnabled true. Hand the URL to the user; the MCP does not email it. Past reports: list_reports and get_report; delete_report removes one.
- "Remove a site" / "offboard a client" -> delete_domain. Irreversible and it releases the site seat; confirm with the user first.

Content Workflows are managed SEO pipelines; when a request matches any scenario below, call list_workflows and pick the workflow whose description fits rather than improvising the analysis yourself:
- "Check my Search Console for growth opportunities" -> workflows that audit search data, plan fixes, and rank near-page-one pages by upside. (For raw Search Console numbers without analysis, the direct get_gsc_* tools above are cheaper and faster.)
- "Where do I rank for X?" / "why do those pages outrank me?" -> live SERP position checks and teardowns of every ranking page.
- "Who are my competitors?" -> competitor discovery and sizing.
- "What should I write about?" / "build a content plan" -> start_workflow on content-plan with runConfig.source set to keywords (seed query or empty seeds to start from the domain), competitor (competitorDomain optional), or ai-gaps (needs AI prompt runs). Keywords mode accepts seedKeywords as a typed list, a previous-run ref, or a keyword-search ref (source keyword-search, searchId, optional selection) from a stored Keyword Ideas search. A content plan artifact is organized as topics. Each topic has a name, an optional folder slug, and one or more pages. A topic may include one pillar page plus supporting pages. Every page has a title, target keywords, and an action: new, refresh, rewrite, consolidate, drop, or skip. Read the plan artifact from get_workflow_result directly; do not reconstruct it from keyword tables.
- "Write an article about X" -> full drafts with outline, FAQs, schema, and links.
- "AI answers skip us" / "close our AI visibility gaps" -> content-plan with runConfig.source ai-gaps: finds prompts where AI cites competitors instead of the brand, turns fan-out searches into a site-aware content plan, and assigns off-site citation actions.
- "Improve an existing page" -> refresh the page you already rank with.
- "Find backlink opportunities" -> sites linking to competitors but not you.
- Local SEO -> Google Maps 5×5 grid scans vs nearby rivals (use scan_map_grid, not start_workflow), a Local SEO audit of the Google Business Profile / reviews / service-area pages (start_workflow on local-audit), AI-citable service-area page copy grounded in the business profile and its reviews.

## Running a workflow

list_workflows to find the right workflow (names, descriptions, credit costs), get_workflow for its runConfigSchema (build runConfig from the input declarations; most fields have defaults), get_workflow_credits to confirm balance (the same pool search_keywords draws on), start_workflow to launch. Poll get_workflow_run until status is 'success', then get_workflow_result for the output document.

Examples:
- "check my Search Console for growth opportunities" -> list_workflows -> get_workflow on the Search Console audit workflow -> start_workflow -> poll get_workflow_run -> get_workflow_result.
- "write pages for the areas we serve" -> list_workflows -> get_workflow on the service-area pages workflow -> start_workflow -> poll -> get_workflow_result.
- "ChatGPT never mentions us" -> list_competitors and list_citations to size the gap -> list_workflows -> the AI-visibility gap content plan -> start_workflow -> poll -> get_workflow_result.
