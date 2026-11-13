# Spurvik Skills

Agent Skills that add EU AI Act Art. 50 transparency notices, zero-prompt-text HMAC call logging and human approval gates to your AI app. They work in Claude Code, Cursor, Codex, Lovable, Bolt, Replit and any agent that reads `SKILL.md`.

```bash
npx skills add spurvik/skills
```

Then ask your agent: *"Use the eu-ai-act skill on this project."*

| Skill | What your agent does |
| --- | --- |
| [`eu-ai-act`](skills/eu-ai-act/SKILL.md) | Adds an Art. 50(1) AI notice, logs each AI call as HMAC-SHA256 fingerprints (never the prompt or output text), and gates send/pay/delete tool calls behind a human. Covers web apps, chatbots, Shopify, WordPress, Webflow, Lovable, Bolt, Next.js, FastAPI and n8n. |

No account or API key needed for the local setup.

## Just want the notice? One line

```html
<script src="https://spurvik.com/badge.js" data-system="Support Assistant" defer></script>
```

No cookies, no network telemetry, 24 EU languages, Shadow DOM. Works in plain HTML, Shopify, WordPress, Webflow, Lovable and Bolt.

`data-attribution` defaults to `true`, which shows a small "Art. 50 · Spurvik" trust link after the notice (Free). Set `data-attribution="false"` to hide it for white-label on Pro (€19/mo) or Done-For-You (€199).

- **WordPress plugin (no code):** [spurvik-eu-ai-act-wp.zip](https://spurvik.com/r/spurvik-eu-ai-act-wp.zip), via Plugins, Add New, Upload Plugin.
- **Framer component:** [spurvik-framer-badge.tsx](https://spurvik.com/r/spurvik-framer-badge.tsx), pasted into Assets, Code, New Code File.
- **shadcn / v0 registry:** `npx shadcn@latest add https://spurvik.com/r/article-50-badge.json`

## SDK

```bash
npm i spurvik
```

`withSpurvik(client, config)` wraps OpenAI, Anthropic and Gemini clients. `spurvikMiddleware` from `spurvik/ai` plugs into the Vercel AI SDK, with adapters for Mastra, OpenAI Agents and LangGraph. It records each call with a keyed HMAC receipt; the prompt and output text are not sent. MIT, source at [spurvik/spurvik](https://github.com/spurvik/spurvik).

## MCP server

Listed in the official MCP Registry as `com.spurvik/eu-ai-act`. The remote server at `https://console.spurvik.com/api/mcp` has 7 tools and needs no API key:

| Tool | What it does |
| --- | --- |
| `spurvik_scan_live_url` | Scans one public HTTPS page for AI chat widgets and SDKs, checks for a visible Art. 50(1) notice and returns a one-line fix |
| `spurvik_classify_ai_act_risk` | Self-assessment from a few answers: prohibited, high-risk (Annex III), Art. 50 only, minimal or outside the EU. Not legal advice; you still decide your tier |
| `spurvik_generate_art50_badge` | Copy-paste Art. 50 notice (badge.js tag or React snippet) in any of the 24 EU languages |
| `spurvik_setup_snippet` | SDK setup for OpenAI, Anthropic, Gemini, Vercel AI SDK, Mastra, OpenAI Agents, LangGraph, React |
| `spurvik_find_unwrapped_calls` | Heuristic scan of pasted code for AI calls not wrapped with Spurvik |
| `spurvik_explain_receipt` | Explains (and optionally verifies) an HMAC call receipt |
| `spurvik_marking_hooks` | How to plug your own Art. 50(2) marking into the SDK |

```bash
# Remote (Streamable HTTP)
claude mcp add --transport http spurvik https://console.spurvik.com/api/mcp

# Local stdio, adds a workspace directory scan for unwrapped AI calls
npx -y spurvik-mcp-server
```

## Try it

- Sandbox: https://console.spurvik.com/sandbox
- Free EU AI Act risk check: https://console.spurvik.com/risk-check
- Free Art. 50(2) C2PA checker: https://console.spurvik.com/c2pa
- Workspace (Frankfurt, fra1): https://console.spurvik.com/signup
- Plans: https://spurvik.com/pricing

| Plan | Price | Adds |
| --- | --- | --- |
| Free | €0 | Notice badge, SDK, MCP server |
| Pro | €19/mo | Hosted Slack/Teams approval queue, 90-day log retention in Frankfurt, DPO engineering summary download |
| Done-For-You 24h Setup | €199 one-off | We wire it into your app within 24 hours. Includes 1 full year of Pro (€190 value). [Buy now](https://console.spurvik.com/signup?plan=concierge) |

## Scope

Spurvik helps you add Art. 50(1) notices and, for high-risk AI systems, Art. 12 logging and Art. 14 human oversight. It does not classify your use case or make your system meet the EU AI Act; that responsibility stays with you as provider or deployer. It does not mark content for Art. 50(2).

## License

MIT © EyeTea Solutions ApS
