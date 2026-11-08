---
name: mcp-listing-pages
description: Publish an MCP server, agent tool or AI product to the directories that build a listing page from a public GitHub README — LobeHub, Glama, Smithery, mcp.so, PulseMCP, the official MCP Registry and awesome lists — so the listing passes dofollow links to a homepage and signup page. Use when the user wants visibility, backlinks or distribution for an MCP server, mentions LobeHub, DR 75, "list my MCP server", "MCP directory", "publish my server", "directory backlinks", or wants a repo README turned into a landing page.
---

# MCP Listing Pages

A directory listing is a second landing page for the product. Most MCP directories build that page from a public GitHub repo, so the README is the asset: every link in it is rendered on the listing, and on LobeHub those links are dofollow.

## The shape of the play

1. One public repo per server, with a README written as a landing page.
2. Two files in the repo root: `lhm.plugin.json` for LobeHub, `server.json` for the official registry.
3. Publish to LobeHub first, then syndicate to the aggregators that crawl GitHub.
4. Keep the README as the single source of truth; update listings by updating the repo.

## Write the README first

This is the page that gets cloned. Order:

- One sentence: what it does and who it is for.
- Install line (`npm i -g ...` or the remote URL) and a three-line quickstart.
- Table of exposed tools: name plus one line each.
- Screenshot or 20-second demo GIF.
- Links, early: homepage, signup, docs, pricing. Absolute, clean URLs.
- License plus one line: works with Claude Code, Cursor, Codex.

Every heading and link is copied onto the directory page. A README with no links produces a listing with no links.

Rules:

- One homepage link, one signup link. Do not repeat the same URL.
- No badge wall above the fold; it dilutes links and bounces humans.
- Keep the description to two sentences; directories truncate.

## lhm.plugin.json

Minimal, repo root:

```json
{
  "identifier": "yourname-your-mcp",
  "name": "Your MCP",
  "version": "0.1.0",
  "description": "One sentence.",
  "homepage": "https://yoursite.com",
  "cloudEndpoint": "https://yoursite.com/mcp",
  "tags": ["your", "tags"]
}
```

`identifier` is stable and marketplace-wide; choose it before the first publish and never change it. `homepage` and `cloudEndpoint` are the fields that point at the site. Both must be absolute URLs.

## Publish to LobeHub

Node 22+. The `npx` form needs no global install (`npm i -g @lobehub/market-cli` gives the `lhm` shorthand).

```bash
npx -y @lobehub/market-cli auth status --output json
npx -y @lobehub/market-cli login           # browser, the user completes it
npx -y @lobehub/market-cli github connect  # browser, proves repo ownership
npx -y @lobehub/market-cli plugin init --url https://yoursite.com/mcp --dir .
npx -y @lobehub/market-cli plugin publish https://github.com/you/repo --dir .
npx -y @lobehub/market-cli plugin list --output json
```

- `login` and `github connect` are human-in-the-loop. Run in the foreground, hand the printed URL to the user, wait for exit, then confirm with `auth status` / `github status`.
- The account must own the repo or have push access.
- Existing listing: edit README, push, bump `version`, run `plugin update --dir <absolute path>`. Same version merges in place; a new version is a release.
- The page is `lobehub.com/mcp/<identifier>`; README links render on it.

## Then the aggregators

| Site | How it gets in | Notes |
|---|---|---|
| Glama | auto-crawls public MCP repos; claim the page | README rendered as-is |
| Smithery | `npx @smithery/cli` or the site submit form | own config, imports the repo |
| mcp.so | submit form with the repo URL | README and links |
| PulseMCP | submit form | curated, slower |
| MCP Registry | publish `server.json` with `mcp-publisher` | canonical source, feeds other directories |
| awesome-mcp-servers | one PR adding the repo | no page, but a link |

LobeHub is the highest-DR page fully under owner control, so do it first. The rest reuse the same README: one good README covers all of them.

## Order of operations

1. Commit README and manifest.
2. Publish to LobeHub; open the live page and confirm the homepage and signup links render.
3. Registry and aggregators.
4. Monthly: bump version, refresh the description, check that links still resolve.

## Do not

- Point the listing homepage at `github.com/...`; use the product domain.
- Submit a repo with a placeholder README; re-crawls are slow.
- Publish the same repo under two accounts; identifiers collide.
- Pay a "directory submission service" for the six rows above.
