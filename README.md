# Spurvik Skills

[![EU AI Act Art. 50](https://console.spurvik.com/api/v1/badge/status.svg?domain=spurvik.com)](https://console.spurvik.com/report?domain=spurvik.com)

Agent Skills that add EU AI Act Art. 50 transparency notices, zero-prompt-text HMAC call logging and human approval gates to your AI app. They work in Claude Code, Cursor, Codex, Lovable, Bolt, Replit and any agent that reads `SKILL.md`.

```bash
npx skills add spurvik/skills
```

Then ask your agent: *"Use the eu-ai-act skill on this project."*

## Check a site, a repo or an image first (zero install, no API key)

```bash
npx -y spurvik@1.0.9 scan yourdomain.com   # AI chat widgets + Art. 50(1) notice status, with a one-line fix
npx -y spurvik@1.0.9 audit .               # unwrapped OpenAI / Anthropic / Gemini / AI SDK calls as file:line
```

Add `--json` for machine output and `--fail-on-gap` to fail a CI job on a gap. `audit` runs locally and prints only `file:line` and the provider, never source text.

Check one public AI image (PNG, JPEG or WebP, up to 4 MB) for an embedded C2PA manifest and its signature:

```bash
curl -sS -X POST https://console.spurvik.com/api/v1/provenance/public-check-url \
  -H "content-type: application/json" \
  -d '{"url":"https://yourdomain.com/hero.jpg"}'
```

It returns JSON (`manifestFound`, `signatureMatches`, `trustAnchor`), sets no cookies and stores nothing; 10 checks a minute per IP.

| Skill | Ask your agent | What your agent does |
| --- | --- | --- |
| [`eu-ai-act`](skills/eu-ai-act/SKILL.md) | *"Use the eu-ai-act skill on this project."* | Adds an Art. 50(1) AI notice, logs each AI call as HMAC-SHA256 fingerprints (never the prompt or output text), and gates send/pay/delete tool calls behind a human. Covers web apps, chatbots, Shopify, WordPress, Webflow, Framer, Lovable, Bolt, Next.js, FastAPI and n8n. |
| [`c2pa-provenance-check`](skills/c2pa-provenance-check/SKILL.md) | *"Check our AI images for C2PA before 2 December 2026."* | Finds AI image generation calls and hero media, checks live image URLs for an embedded C2PA manifest and for CDN stripping (`/_next/image`, Cloudflare Polish, Imgix, Cloudinary), wires your own Art. 50(2) marking into the SDK hook, and adds a visible "AI-generated" label. |
| [`agency-ai-act-audit`](skills/agency-ai-act-audit/SKILL.md) | *"Audit these 10 client sites for AI chat widgets."* | Batch-scans 1 to 10 client sites for AI chat widgets (Intercom Fin, Zendesk AI, Tidio Lyro, HubSpot Breeze, Gorgias, Crisp, Drift, Dixa) with no visible Art. 50(1) notice, then writes a client-ready table with report links, status shields and the right install per platform. |

No account or API key needed for the local setup.

## Just want the notice? One line

```html
<script src="https://spurvik.com/badge.js" data-system="Support Assistant" defer></script>
```

No cookies, no network telemetry, 24 EU languages, Shadow DOM. Works in plain HTML, Shopify, WordPress, Webflow, Lovable and Bolt.

`data-attribution` defaults to `true`, which shows a small "Art. 50 · Spurvik" trust link after the notice (Free). Set `data-attribution="false"` to hide it for white-label on Pro (€19/mo) or Done-For-You (€199).

- **Shopify (Online Store 2.0 section, no app):** [spurvik-shopify-art50.liquid](https://spurvik.com/r/spurvik-shopify-art50.liquid). Save it as `sections/spurvik-art50-notice.liquid`, then add the "Spurvik Art. 50 notice" section to the footer. Language Auto maps `request.locale.iso_code` to the 24 EU languages; the position defaults to `bottom-left` so chat launchers on the right stay clear.
- **WordPress plugin v1.1.0 (no code):** [spurvik-eu-ai-act-wp.zip](https://spurvik.com/r/spurvik-eu-ai-act-wp.zip), via Plugins, Add New, Upload Plugin. Add `[spurvik_status_shield]` to a footer widget or Shortcode block for a live status shield.
- **Framer component:** [spurvik-framer-badge.tsx](https://spurvik.com/r/spurvik-framer-badge.tsx), pasted into Assets, Code, New Code File.
- **shadcn / v0 registry:** `npx shadcn@latest add https://spurvik.com/r/article-50-badge.json`

## SDK

```bash
npm i spurvik
```

`withSpurvik(client, config)` wraps OpenAI, Anthropic and Gemini clients. `spurvikMiddleware` from `spurvik/ai` plugs into the Vercel AI SDK, with adapters for Mastra, OpenAI Agents and LangGraph. It records each call with a keyed HMAC receipt; the prompt and output text are not sent. MIT, source at [spurvik/spurvik](https://github.com/spurvik/spurvik).

## MCP server

Listed in the official MCP Registry as `com.spurvik/eu-ai-act`. Neither server needs an API key. The remote server at `https://console.spurvik.com/api/mcp` has 9 tools; the local `spurvik-mcp-server@1.0.6` has the first 8 (no `spurvik_check_image_c2pa_url`):

| Tool | What it does |
| --- | --- |
| `spurvik_scan_live_url` | Scans one public HTTPS page for AI chat widgets and SDKs, checks for a visible Art. 50(1) notice and returns a one-line fix |
| `spurvik_batch_scan_domains` | Scans 1 to 10 public domains in parallel (for example an agency's client sites). Returns the widget and SDK gaps, a `badge.js` fix in each site's language (24 EU languages), a shareable `/report?domain=` brief URL and a live status shield in Markdown per domain |
| `spurvik_classify_ai_act_risk` | Self-assessment from a few answers: prohibited, high-risk (Annex III), Art. 50 only, minimal or outside the EU. You still decide your tier |
| `spurvik_generate_art50_badge` | Copy-paste Art. 50 notice (badge.js tag or React snippet) in any of the 24 EU languages |
| `spurvik_setup_snippet` | SDK setup for OpenAI, Anthropic, Gemini, Vercel AI SDK, Mastra, OpenAI Agents, LangGraph, React |
| `spurvik_find_unwrapped_calls` | Heuristic scan of pasted code for AI calls not wrapped with Spurvik |
| `spurvik_explain_receipt` | Explains (and optionally verifies) an HMAC call receipt |
| `spurvik_marking_hooks` | How to plug your own Art. 50(2) marking into the SDK |
| `spurvik_check_image_c2pa_url` | Remote only. Checks one public https image URL (PNG, JPEG or WebP, up to 4 MB) for an embedded C2PA manifest, its signature and trust list result, and image optimizers that strip it (`/_next/image`, Cloudinary, Imgix, Cloudflare). Read-only, nothing stored, 10 checks a minute |

```bash
# Remote (Streamable HTTP)
claude mcp add --transport http spurvik https://console.spurvik.com/api/mcp

# Local stdio, adds a workspace directory scan for unwrapped AI calls
npx -y spurvik-mcp-server@1.0.6
```

## Try it

- Sandbox: https://console.spurvik.com/sandbox
- Free EU AI Act risk check: https://console.spurvik.com/risk-check
- Free Art. 50(2) C2PA manifest and CDN-strip checker: https://console.spurvik.com/c2pa
- Free agency batch scanner (up to 10 domains): https://console.spurvik.com/portfolio
- Workspace (Frankfurt, fra1): https://console.spurvik.com/signup
- Plans: https://spurvik.com/pricing

| Plan | Price | Adds |
| --- | --- | --- |
| Free | €0 | Notice badge, SDK, MCP server |
| Pro | €19/mo (€190/yr) | Hosted Slack/Teams approval queue, 90-day log retention in Frankfurt, DPO engineering summary download, white-label badge (`data-attribution="false"`) |
| Team | €79/mo (€790/yr) | Everything in Pro, 2,000,000 events a month, 12-month log retention, white-label client projects for agencies |
| Done-For-You 24h Setup | €199 once | We wire it into your app within 24 hours. Includes 1 full year of Pro (€190 value). [Buy now](https://console.spurvik.com/signup?plan=concierge) |

Prices excl. VAT, B2B.

## License

MIT © EyeTea Solutions ApS

<sub>Scope: Spurvik supports Art. 50(1) notices and, for high-risk systems, Art. 12 logging and Art. 14 oversight. It does not classify your use case, sign or mark content for Art. 50(2), or make your system meet the EU AI Act. Not legal advice.</sub>
