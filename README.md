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

## SDK

```bash
npm i spurvik
```

`withSpurvik(client, config)` wraps OpenAI, Anthropic and Gemini clients. `spurvikMiddleware` from `spurvik/ai` plugs into the Vercel AI SDK, with adapters for Mastra, OpenAI Agents and LangGraph. It records each call with a keyed HMAC receipt; the prompt and output text are not sent. MIT, source at [spurvik/spurvik](https://github.com/spurvik/spurvik).

## MCP server

Listed in the official MCP Registry as `com.spurvik/eu-ai-act`.

```bash
# Remote (Streamable HTTP)
claude mcp add --transport http spurvik https://console.spurvik.com/api/mcp

# Local stdio, adds a workspace scan for unwrapped AI calls
npx -y spurvik-mcp-server
```

## Try it

- Sandbox: https://console.spurvik.com/sandbox
- Workspace (Frankfurt, fra1): https://console.spurvik.com/signup
- Plans: https://spurvik.com/pricing

| Plan | Price | Adds |
| --- | --- | --- |
| Free | €0 | Notice badge, SDK, MCP server |
| Pro | €19/mo | Hosted Slack/Teams approval queue, 90-day log retention in Frankfurt, DPO engineering summary download |
| Done-For-You 24h Setup | €199 one-off | We wire it into your app within 24 hours. Email hello@spurvik.com |

## Scope

Spurvik helps you add Art. 50(1) notices and, for high-risk AI systems, Art. 12 logging and Art. 14 human oversight. It does not classify your use case or make your system meet the EU AI Act; that responsibility stays with you as provider or deployer. It does not mark content for Art. 50(2).

## License

MIT © EyeTea Solutions ApS
