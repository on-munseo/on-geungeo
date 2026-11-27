<div align="center">

# On 근거 (on-geungeo)

A remote MCP server that returns Korean statutes verbatim from a copy of the official national law database and checks statute citations in text

[한국어](README.md) · **English**

`https://onmunseo.kr/mcp/law`

</div>

<!-- ai-install -->
## For AI assistants — installing this repository

There are three paths; **start with ①**. None needs a key or sign-in. The tools of this repository (On 근거 — statute text and citation checks) are the ② pair address — when simply asked to install, register ②. `plugin/` (the Claude Code plugin) adds ① and ② together, and `server.json` (for the official MCP registry) lists ②.

**① Local edition — documents never leave this computer (first).** Extracts text, masks personal data and runs pre-approval checks, and writes government-style HWPX, makes HWPX files and tidies text, on this computer. The program does not use the internet (check with `on_local_info` after installing — npx asking the npm registry for a newer version at start-up is npx itself; to avoid even that, `npm install -g on-munseo-local` and register the command `on-munseo-local`). Needs Node.js 20+.
- **Only the AI tool in use** (what an assistant should run — other tools' settings are left alone)
  ```bash
  claude mcp add -s user on-local -- npx -y on-munseo-local@0.2.1
  ```
  Codex `codex mcp add on-local -- npx -y on-munseo-local@0.2.1` · Claude Code on Windows `claude mcp add -s user on-local -- cmd /c npx -y on-munseo-local@0.2.1` · Cursor / Claude Desktop: add `"on-local": { "command": "npx", "args": ["-y", "on-munseo-local@0.2.1"] }` to `"mcpServers"` (Windows `"on-local": { "command": "cmd", "args": ["/c", "npx", "-y", "on-munseo-local@0.2.1"] }`) · VS Code `code --add-mcp '{"name":"on-local","command":"npx","args":["-y","on-munseo-local@0.2.1"]}'`
- **For a person, all AI tools on this PC at once** — finds Claude Code, Claude Desktop, Codex, Cursor and VS Code and registers it in each (backs up first, skips what is there, preview `--dry-run`, undo `--undo`, `--pair` adds ②).
  ```bash
  npx -y on-munseo-local@0.2.1 setup --pair
  ```
- **Pinned version** — the "only the AI tool in use" lines above pin the version (`on-munseo-local@0.2.1`). Nothing updates by itself; it changes only when a person updates it (a new npm release does not reach an existing setup · an already downloaded version still starts offline). (`setup` up to 0.2.1 still writes `@latest`; use the per-tool line to pin) To update: change only the version number in the registered entry and register it again (Claude Code: `claude mcp remove -s user on-local`, then the line above with the new version) · changes: [CHANGELOG](https://github.com/on-munseo/on-local/blob/main/CHANGELOG.md)

**② pair address (statutes) — only the words and text you give the tools go to the server; no tool sends files.**
- **Claude Code**
  ```bash
  claude mcp add --transport http -s user on-geungeo https://onmunseo.kr/mcp/law
  ```
- **Codex** — append two lines to the config file (`~/.codex/config.toml`; skipped if already there). Do not use `codex mcp add --url` — it opens a browser sign-in window right away.
  ```bash
  C="${CODEX_HOME:-$HOME/.codex}"; mkdir -p "$C"; touch "$C/config.toml"; if ! (cd / && codex mcp get on-geungeo >/dev/null 2>&1); then if cp "$C/config.toml" "$C/config.toml.bak-on-geungeo"; then printf '\n[mcp_servers.on-geungeo]\nurl = "https://onmunseo.kr/mcp/law"\n' >> "$C/config.toml"; (cd / && codex mcp get on-geungeo >/dev/null 2>&1) || { cp "$C/config.toml.bak-on-geungeo" "$C/config.toml"; echo "자동으로 더하지 못했습니다 — config.toml 에 손으로 더해 주세요"; }; else echo "백업을 만들지 못해 그만둡니다 — config.toml 은 그대로입니다"; fi; fi; (cd / && codex mcp get on-geungeo)
  ```
  On Windows, or where the `codex` command is not installed (IDE extension only), add the two lines `[mcp_servers.on-geungeo]` and `url = "https://onmunseo.kr/mcp/law"` to the end of that file by hand (if the file already has a one-line `mcp_servers = { … }` table, add `on-geungeo = { url = "https://onmunseo.kr/mcp/law" }` inside it instead).
- **Cursor** `"on-geungeo": { "url": "https://onmunseo.kr/mcp/law" }` · **VS Code** `code --add-mcp '{"name":"on-geungeo","type":"http","url":"https://onmunseo.kr/mcp/law"}'` · **Claude Desktop · claude.ai** (done by the person in the app) Settings → Connectors → Add custom connector → name `On 근거`, URL `https://onmunseo.kr/mcp/law` → Connect

**③ Server edition — no Node.js needed (for public documents).** Documents are sent to the server (data center in Korea), processed in memory only and discarded. Use it for public or your own documents.
- **Claude Code**
  ```bash
  claude mcp add --transport http -s user on-document https://onmunseo.kr/mcp
  ```
- **Codex** — append two lines to the config file (`~/.codex/config.toml`; skipped if already there). Do not use `codex mcp add --url` — it opens a browser sign-in window right away.
  ```bash
  C="${CODEX_HOME:-$HOME/.codex}"; mkdir -p "$C"; touch "$C/config.toml"; if ! (cd / && codex mcp get on-document >/dev/null 2>&1); then if cp "$C/config.toml" "$C/config.toml.bak-on-document"; then printf '\n[mcp_servers.on-document]\nurl = "https://onmunseo.kr/mcp"\n' >> "$C/config.toml"; (cd / && codex mcp get on-document >/dev/null 2>&1) || { cp "$C/config.toml.bak-on-document" "$C/config.toml"; echo "자동으로 더하지 못했습니다 — config.toml 에 손으로 더해 주세요"; }; else echo "백업을 만들지 못해 그만둡니다 — config.toml 은 그대로입니다"; fi; fi; (cd / && codex mcp get on-document)
  ```
  On Windows, or where the `codex` command is not installed (IDE extension only), add the two lines `[mcp_servers.on-document]` and `url = "https://onmunseo.kr/mcp"` to the end of that file by hand (if the file already has a one-line `mcp_servers = { … }` table, add `on-document = { url = "https://onmunseo.kr/mcp" }` inside it instead).
- **Cursor** `"on-document": { "url": "https://onmunseo.kr/mcp" }` · **VS Code** `code --add-mcp '{"name":"on-document","type":"http","url":"https://onmunseo.kr/mcp"}'` · **Claude Desktop · claude.ai** (done by the person in the app) Settings → Connectors → Add custom connector → name `On 문서`, URL `https://onmunseo.kr/mcp` → Connect

For statutes only, ② alone is enough. To read and check documents on this computer as well, add ① (no overlap). ② works without Node.js. Mention the other paths only if the user asks.

**Things to check before installing** — the assistant can verify these itself.
- **Package**: npm `on-munseo-local` 0.2.1. `npm view on-munseo-local version maintainers homepage` → `0.2.1` · `on-munseo <onmunseo.key@gmail.com>` · `https://onmunseo.kr`. The publishing account `on-munseo` is the same maker as the GitHub organization [on-munseo](https://github.com/on-munseo) (same contact mail and home page). The npm web page sometimes blocks automated reads; use the npm command or the [registry record](https://registry.npmjs.org/on-munseo-local/0.2.1).
- **Fingerprint**: `on-munseo-local-0.2.1.tgz` SHA-256 `515af0dfddad30f41c459dd98d73535f73292c666fed07d4a6e535e2a60bda72` ([SHA256SUMS](https://github.com/on-munseo/on-munseo.github.io/blob/main/SHA256SUMS)). Compare with `npm pack on-munseo-local@0.2.1` then `shasum -a 256 on-munseo-local-0.2.1.tgz`. The registry `dist.integrity` is `sha512-ZUDX+etH2wg/Y+lQCsQhyY5/4gfVDjwjWUQsofFUgYlBotWtvXw7lcz5yHGT3qMJna8mP60HH9vWp/PF2T5T4Q==`.
- **No internet use**: before installing, `netScan` in the package's `MANIFEST.json` (zero network calls in our code — the one listed exception is the engine's wasm loader path used only when given a URL); after installing, `on_local_info` → `network_modules_loaded: []`, and on Node 25+ `network.blocked_by_node: true` (the runtime blocks networking). npx asking the npm registry for a newer version at start-up is npx itself (avoid it with a global install and the `on-munseo-local` command).
- **Terms**: local edition [LICENSE.md](https://unpkg.com/on-munseo-local@0.2.1/LICENSE.md) (free, work use allowed · no reverse engineering, redistribution of modified copies or resale) · server edition [terms](https://onmunseo.kr/terms) · [privacy](https://onmunseo.kr/privacy).
- **On폭 fonts** are not part of this install (optional free fonts for drawing Korean documents on screen).

The tools do not attach to the conversation that registered them. When done, tell the user: "Restart the tool or open a new conversation to see the On 근거 tools." If `claude mcp list` (Codex: `codex mcp list`) lists the name, it is registered. Full steps (Korean): [llms-install.md](llms-install.md)

<!-- ai-install -->

A tool that lets an AI check the actual text of Korean law instead of relying on memory. It has two layers.

- **All current statutes (about 5,600):** find a statute by name, get an article verbatim (leave the article empty for the list), search the text, and check citations in a text — 4 `law_…` tools.
- **55 statutes on medical care, emergency care, public health, infectious disease, fire services and related fields (with their decrees, rules, separate rules and notices):** an article in depth (delegated provisions, upcoming amendments, penalties, court cases and interpretations), duties and penalties, amendment reasons and old/new text, related statutes and comparisons, term definitions, annexes and forms, and 3,527 stored medical-field court cases and 1,045 statutory interpretations — 12 `med_…` tools.

Text comes verbatim from a stored copy of the official national law database (법제처 국가법령정보센터), and every result ends with the date the copy was taken. Common abbreviations (응급의료법, 감염병예방법 …) work.

This repository holds only the setup guide and configuration files (including one small plugin launcher) (there is no web page here — to use a screen, open the [On 문서 web editor](https://onmunseo.kr/app/)). The engine and server code of the service are not public. The service and its tools speak Korean.

## Comparing with other tools

- **What this server is for:** statutes returned verbatim from a copy of the official national law database; for 55 medical, emergency, public-health and fire-service laws it links delegated provisions, upcoming amendments, reasons for amendments, old/new comparisons, duties and penalties, annexes, 3,527 medical court cases and 1,045 statutory interpretations; it checks statute citations in a text (missing laws, articles, paragraphs or items, deleted articles, wrong article titles) without AI (`law_check_citations`).
- **Where a statute-lookup MCP that calls the national law API in real time goes further:** local ordinances, the body of administrative rules, and court cases outside medicine. This server uses a downloaded copy, so later amendments arrive with the next copy (the download date is shown at the end of each result).

**Jump to:** [Comparing](#comparing-with-other-tools) · [Why](#in-short--why-use-it) · [Tools](#what-it-does) · [Connect](#connect) · [Examples](#examples) · [Measured results](#measured-results) · [Guest and key](#guest-and-key) · [Safety](#safety) · [Good to know](#good-to-know)

## In short — why use it

| Common pain | With On 근거 |
|---|---|
| An AI makes up plausible articles or quotes old versions | Text comes verbatim from a copy of the official database, with the date it was taken |
| You look up every “Act, Article N(K)” citation in a draft by hand | `law_check_citations` flags missing statutes, articles, paragraphs or items, deleted articles and wrong titles (rule-based, up to 40 at a time) |
| You search separately for an article’s delegated provisions, penalties and upcoming changes | `med_article` shows them together (for the 55 medical and emergency statutes) |
| The same term is defined differently in different statutes | See them side by side with `med_terms` and `med_compare` |

**When you do not need it:** When a person reads a single statute, the national law database (law.go.kr) is the most accurate and up to date. This tool exists so that an AI checks the original text.

## What it does

| Tool | What it does | Guest |
|---|---|---|
| `law_find` | Finds current statutes (about 5,600 acts, decrees and rules) by name, abbreviations included; administrative rules are listed by name only | yes |
| `law_article` | The article text, verbatim, from any current statute; leave the article empty for the list of article titles | yes |
| `law_search` | Searches one statute’s text (spacing ignored), or all statute names and article titles | yes |
| `law_check_citations` | Checks statute citations in a text against the current text: missing statutes, articles, paragraphs or items, deleted articles, wrong titles (up to 40 at a time) | yes |
| `med_laws` | The medical and emergency statutes covered, by field (with abbreviations) | yes |
| `med_article` | Article text + upcoming amendments + delegated provisions + penalties + cases and interpretations that cite it | yes |
| `med_search` | Searches the covered acts, decrees and rules | yes |
| `med_duties` | Duties, prohibitions and deadlines of an article, with penalty and fine provisions and the relevant annex lines | yes |
| `med_changes` | Amendments of the past year and upcoming ones, the official reasons, and old/new text with changes marked 【 】 | yes |
| `med_upcoming` | Promulgated amendments not yet in force, by effective date | yes |
| `med_related` | Citation links between statutes; for an article, the provisions of other statutes that use it | yes |
| `med_compare` | One topic side by side in 2–4 statutes | yes |
| `med_terms` | How each statute defines the same term | yes |
| `med_checklist` | What a given party (as named in the law) must and must not do, as article lines | yes |
| `med_annex` | Annexes and forms: list, full text, or the part around a word | yes |
| `med_decisions` | Searches the stored medical-field court cases and statutory interpretations; full text by id | yes |

- The 16 guest tools were checked on 2026-11-26 by calling `tools/list` on the live server as a guest.
- Every key and every guest can use them, whatever the key’s permissions. The all-in-one endpoint (`/mcp`) has the same 16 tools.

## Connect

Of the three paths at the top ([For AI assistants](#for-ai-assistants--installing-this-repository): ① local edition · ② pair address · ③ server edition), this section details ②, the pair address of this bundle (statutes), for each tool.

**Claude Code** — one line in the terminal (`-s user` makes it available in every folder):

```bash
claude mcp add --transport http -s user on-geungeo https://onmunseo.kr/mcp/law
```

**Claude Code plugin** — adds ① the local edition + ② the statute pair (Node.js 20+; works on Windows too — the small launcher `plugin/run-local.mjs` starts npx). Installing several of these product plugins starts the local edition several times and shows the same tools more than once, so install just one. One line (Claude Code 2.1.275 or later):

```bash
claude plugin install on-geungeo --marketplace on-munseo/on-geungeo
```

In every app, the conversation in which you register the server does not get the tools yet. Restart the app once (Claude Code: `/exit`, then `claude`; VS Code: a new chat) or open a new conversation, and the tools are there from then on. Without a key, skip the sign-in step and use it as a guest.

> To use all document tools and the law tools (On 근거) together, add `https://onmunseo.kr/mcp` instead of this bundle (recommended — [On 문서](https://github.com/on-munseo/on-munseo.github.io)). If you add both, the same tools appear twice.

### Claude Code — more

- With a key: in Claude Code open `/mcp` → `on-geungeo` → Authenticate, and the On 문서 sign-in page opens. Enter your key there.
- To pass the key as a header: `claude mcp add --transport http -s user on-geungeo https://onmunseo.kr/mcp/law --header "Authorization: Bearer <key>"`. This stores the key in `~/.claude.json`, so the sign-in page is the better way.
- Two-step plugin install (older versions): `claude plugin marketplace add on-munseo/on-geungeo`, then `claude plugin install on-geungeo@on-geungeo`. In a Claude Code chat: `/plugin marketplace add on-munseo/on-geungeo` → `/plugin install on-geungeo@on-geungeo`.
- Check: `claude mcp list` · remove: `claude mcp remove on-geungeo`.

### claude.ai · Claude Desktop (connectors)

Settings → Connectors → “Add custom connector”: name `On 근거`, URL `https://onmunseo.kr/mcp/law`, then Connect. With a key, enter it on the On 문서 sign-in page that opens; without one you are a guest. Menu names differ by plan and version, and some plans may not have this menu.

To add it to Claude Desktop through the config file, use the `mcp-remote` bridge (needs Node.js). macOS `~/Library/Application Support/Claude/claude_desktop_config.json` · Windows `%APPDATA%\Claude\claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "on-geungeo": {
      "command": "npx",
      "args": [
        "-y",
        "mcp-remote",
        "https://onmunseo.kr/mcp/law"
      ]
    }
  }
}
```

To send a key as a header, append `"--header", "Authorization:${ON_AUTH}"` to `"args"` and add `"env": { "ON_AUTH": "Bearer <key>" }` (no space after the colon, so Windows does not split the argument). Restart Claude Desktop after saving.

### ChatGPT

1. Settings → Apps & Connectors → Advanced settings: turn on Developer mode (wording may differ by version).
2. Create a custom MCP server: name `On 근거`, URL `https://onmunseo.kr/mcp/law`, authentication “OAuth” if you have a key, otherwise “None”.
3. Enter your key on the On 문서 sign-in page, then turn the connector on from the tools (+) menu of a chat.

- ChatGPT receives a separate pass instead of your key (same permissions and daily quota). Revoking the key blocks the pass at once.

### Codex

Without a key, add two lines to `~/.codex/config.toml`:

```toml
[mcp_servers.on-geungeo]
url = "https://onmunseo.kr/mcp/law"
```

- `codex mcp add` opens the On 문서 sign-in window right away. With a key, enter it there; without one, close it (Ctrl-C) and use the two lines above.

### Cursor

`~/.cursor/mcp.json` (all projects) or `.cursor/mcp.json` in a project:

```json
{
  "mcpServers": {
    "on-geungeo": {
      "url": "https://onmunseo.kr/mcp/law"
    }
  }
}
```

If your Cursor version cannot sign in to remote MCP servers (OAuth), add `"headers": { "Authorization": "Bearer <key>" }` (the key is then stored in the file).

### VS Code

`.vscode/mcp.json` in the workspace:

```json
{
  "servers": {
    "on-geungeo": {
      "type": "http",
      "url": "https://onmunseo.kr/mcp/law"
    }
  }
}
```

To send a key as a header without writing it in the file, let VS Code ask for it on first start:

```json
{
  "inputs": [
    {
      "type": "promptString",
      "id": "on-key",
      "description": "On 문서 key",
      "password": true
    }
  ],
  "servers": {
    "on-geungeo": {
      "type": "http",
      "url": "https://onmunseo.kr/mcp/law",
      "headers": {
        "Authorization": "Bearer ${input:on-key}"
      }
    }
  }
}
```

### Try it with curl

List the tools (add `-H "Authorization: Bearer <key>"` if you have a key):

```bash
curl -s https://onmunseo.kr/mcp/law \
  -H "Content-Type: application/json" -H "Accept: application/json, text/event-stream" \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/list"}'
```

Call `law_article` — sending the request from a file keeps quotes and line breaks intact:

```bash
cat > call.json <<'EOF'
{"jsonrpc": "2.0", "id": 1, "method": "tools/call", "params": {"name": "law_article", "arguments": {"law": "응급의료법", "jo": "제2조"}}}
EOF
curl -s https://onmunseo.kr/mcp/law \
  -H "Content-Type: application/json" -H "Accept: application/json, text/event-stream" \
  --data-binary @call.json
```

The reply was Article 2 (Definitions) of the Emergency Medical Service Act, verbatim, ending with the line that the text comes from the national law database copy taken on 2026-11-14, for reference (2026-11-26, guest). The answer is the text in `result.content`. The server is stateless (Streamable HTTP, POST), so a single `tools/call` works without `initialize`. Opening the URL in a browser shows a short guide page.

## Examples

| If you say | This tool |
|---|---|
| “Show me Article 2 of the Emergency Medical Service Act” | `med_article` or `law_article` |
| “Check the statute citations in this draft” | `law_check_citations` |
| “What reporting duties does the Infectious Disease Control Act set, and the penalties?” | `med_duties` |
| “Which amendments to the Emergency Medical Service Act take effect soon?” | `med_upcoming` · `med_changes` |
| “Compare the ambulance provisions of two acts side by side” | `med_compare` |
| “Find the articles of the Personal Information Protection Decree that mention pseudonymous data” | `law_search` |

The AI decides which tool to call; these are typical cases.

## Measured results

The tool returns the law text as is, so there is no accuracy score. Below are the size of the stored data and the dates the copies were taken.

| Data | Size | Copy taken (checked in results on 2026-11-26) |
|---|---|---|
| All current statutes (acts, decrees, rules) | about 5,600 | 2026-11-14 (from the last line of a `law_article` result) |
| Medical and emergency statutes (with decrees, rules and notices) | 55 | 2026-11-23 (from the last line of a `med_article` result) |
| Medical-field court cases · statutory interpretations | 3,527 · 1,045 | stored copy |

Weak spots: this is a stored copy, so later amendments arrive with the next copy (the date is at the end of every result). Administrative rules such as notices and directives can be found by name only; their text is included only where attached to the medical and emergency statutes. Court cases and interpretations come only from the stored medical-field set. Results are for reference; the final word belongs to the official text and the responsible authority’s interpretation.

## Guest and key

|  | Guest (no key) | Key |
|---|---|---|
| How to connect | Add the URL with no authentication | Enter the key on the On 문서 sign-in page (OAuth), or send `Authorization: Bearer <key>` |
| Tools at this endpoint | 16 | 16 |
| Daily quota | 50 calls a day and 6 a minute (the statute tools `law_`·`med_` have their own 100 a day and 20 a minute) per internet address (IP), counted across all On 문서 endpoints and the web editor. When the shared guest quota for the day runs out, guest access closes until midnight Korea time | A daily quota and concurrency limit set per key |
| Needs a key | — | none (every key and guest gets the same 16 tools) |

- To get a key, write to `onmunseo.key [at] gmail.com` (subject “On 문서 열쇠 신청”), with what you will use it for and which bundle you need (e.g. On 근거).
- A wrong or revoked key gives **401**; a bundle that is not in your key gives **403**. Too many calls in a short time give “too many requests” (429) — wait about a minute.
- Never append the key to the URL; it would end up in chat and access logs.

## Safety

1. **Nothing is kept.** Text you send is discarded when done; the access log holds only the key alias, tool name, request size, time taken and success.
2. **Verbatim text.** Articles, cases and interpretations come from the stored copy and are not rewritten by AI; citation checks are rule-based.
3. **Public data only.** It returns only public data from the national law database (statutes, court cases, statutory interpretations).
4. **No bulk harvesting.** Calls that look like scraping are paused for a day (see Good to know).

Report security problems as described in [SECURITY.md](SECURITY.md).

## Good to know

- **This is a free trial service.** Features, quotas and URLs may change or stop. Changes are noted in [CHANGELOG.md](CHANGELOG.md).
- **Results are for reference.** Have a person check them before approval.
- Results over 12,000 characters are cut, with a request to narrow the question.
- Very many calls in a short time, or walking through one statute article by article, pauses the law tools for that key (for guests, that address) for a day. Two requests are handled at a time; the rest wait their turn.
- Law tools count toward the same daily quota.
- Internet only — it cannot be used from closed government networks.
- Menus differ by app and version, so screens may not match the steps above. If something fails, see [SUPPORT.md](SUPPORT.md).

## Terms, privacy and notices

- Bulk collection of results and use of results to train other AI models are not allowed. Automated abuse (several keys to get around limits, non-stop calls, scraping results) leads to the key being cut off without notice. Extracting or reverse-engineering the engine, rules or code is not permitted.
- Summary: [terms in the MCP guide](https://github.com/on-munseo/on-munseo.github.io/blob/main/MCP.md#5-이용-조건약관-요약) (Korean). Full text: [terms of use](https://onmunseo.kr/terms) and [privacy policy](https://onmunseo.kr/privacy) (Korean).
- The statutes, court cases and statutory interpretations returned by On 근거 are public data from the national law database (법제처 국가법령정보센터).
- Claude, ChatGPT, Cursor, VS Code and other names belong to their owners and are used only to explain how to connect. Full notices: [notices in the MCP guide](https://github.com/on-munseo/on-munseo.github.io/blob/main/MCP.md#고지) (Korean).

## Glossary

- **MCP** — Model Context Protocol, an open standard for AI tools to call outside tools. Add this URL as an MCP server and the AI calls these tools inside the chat.
- **Remote MCP server** — An MCP server reached over the internet; nothing to install on your computer.
- **Bundle endpoint** — A URL that carries one On 문서 feature set (`/mcp/text` and so on). Everything is at `/mcp`.
- **Key (열쇠)** — The access value issued by On 문서, with a daily quota and a set of bundles. Keep it secret.
- **Guest (손님)** — Use without a key: some tools and a small daily quota, right away.
- **OAuth (sign-in page)** — You enter the key on the On 문서 sign-in page when connecting; the app receives a separate pass instead of the key.
- **Delegated provision** — The decree or rule article that fills in what an act leaves to it (“as prescribed by Presidential Decree”).
- **Old/new comparison** — The article before and after an amendment side by side, with the changes marked.
- **Statutory interpretation** — The Ministry of Government Legislation’s answer to a question about what a provision means.
- **Administrative rule** — Rules issued by agencies, such as notices, directives and guidelines.

## Other On 문서 bundles

| Product | Repository | Endpoint (after `https://onmunseo.kr`) |
|---|---|---|
| **On 문서 (all)** | [on-munseo.github.io](https://github.com/on-munseo/on-munseo.github.io) — web editor and every MCP tool | `/mcp` |
| On 글꺼내기 | [on-text](https://github.com/on-munseo/on-text) | `/mcp/text` |
| On 결재점검 | [on-check](https://github.com/on-munseo/on-check) | `/mcp/check` |
| On 개인정보 가리기 | [on-mask](https://github.com/on-munseo/on-mask) | `/mcp/mask` |
| On HWPX 만들기 | [on-hwpx](https://github.com/on-munseo/on-hwpx) | `/mcp/hwpx` |
| On 공문서 쓰기 | [on-gov](https://github.com/on-munseo/on-gov) | `/mcp/gov` |
| On 글 다듬기 | [on-tidy](https://github.com/on-munseo/on-tidy) | `/mcp/tidy` |
| On 근거 | this repository | `/mcp/law` |
| On폭 글꼴 (fonts) | [napurrin/onpok-fonts](https://github.com/napurrin/onpok-fonts) — free fonts (SIL OFL 1.1) · not needed for the MCP install (optional) | — |

## License

Text is [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/); configuration files (`server.json`, `.claude-plugin/`, `plugin/`) are MIT. The names “On 문서” and “On 근거” and the logo are not covered by these licenses. The engine and server code of the service are not in this repository and are not public. See [LICENSE](LICENSE).

---

Made by [napurrin](https://github.com/napurrin), who builds and maintains it while learning from Coco.
