# Spurvik Skills

[![EU AI Act Art. 50](https://console.spurvik.com/api/v1/badge/status.svg?domain=spurvik.com)](https://console.spurvik.com/report?domain=spurvik.com)

Four Agent Skills that add EU AI Act Art. 50 transparency notices, zero-prompt-text HMAC call logging and human approval gates to your AI app, check AI images for C2PA, audit client sites, and answer vendor security questionnaires from real evidence. They work in Claude Code, Cursor, Codex, Lovable, Bolt, Replit and any agent that reads `SKILL.md`.

```bash
npx skills add spurvik/skills
```

Then ask your agent: *"Use the eu-ai-act skill on this project."*

## Check a site, a repo or an image first (zero install, no API key)

```bash
npx -y spurvik@1.0.15 setup                                     # auto-detect stack (Next/AI SDK/FastAPI/Shopify/WP/React) & print SDK/badge setup
npx -y spurvik@1.0.15 scan yourdomain.com                       # AI chat widgets + Art. 50(1) notice status (supports 1–10 domains at once)
npx -y spurvik@1.0.15 audit .                                   # unwrapped OpenAI / Anthropic / Gemini / AI SDK / Python calls as file:line
npx -y spurvik@1.0.15 c2pa yourdomain.com                       # 1-hop store hero/og:image C2PA & CDN-strip check (or pass a direct https image URL)
npx -y spurvik@1.0.15 rfp yourdomain.com --plan pro             # offline draft of the 5 EU AI Act vendor questionnaire answers (Q1–Q5)
npx -y spurvik gate yourdomain.com                              # offline 6-vector Art. 14 fail-closed blocking self-test (spurvik/channels)
npx -y spurvik risk yourdomain.com                              # offline EU AI Act risk tier + DPO Self-Assessment (.md) record
```

Add `--json` for machine output. `scan` and `audit` take `--fail-on-gap` to fail a CI job on a gap; `c2pa` takes `--fail-on-missing`, which exits 2 when the image has no C2PA manifest or its signature does not match. `audit` runs locally and prints only `file:line` and the provider, never source text. `c2pa` checks one public https PNG, JPEG or WebP image up to 4 MB (or auto-discovers a store's frontpage hero image in 1 hop from a bare domain), names the image optimizer that strips the manifest (`/_next/image`, Shopify CDN `/cdn/shop/`, Jetpack Photon, Webflow CDN, Cloudinary, Imgix, Cloudflare) and refuses localhost and private-IP URLs before any request. It reads; it never signs.

`rfp` makes no network request. It drafts the same Q1–Q5 Markdown as MCP tool #10 `spurvik_vendor_questionnaire_answers` (in all 8 flagship EU languages — English, German, French, Dutch, Danish, Swedish, Spanish or Italian): `npx -y spurvik@1.0.15 rfp [domain] [--plan free|pro|team|concierge] [--provider openai|anthropic|google-genai|vercel-ai-sdk|langgraph|openai-agents|mastra|python|custom] [--lang en|de|fr|nl|da|sv|es|it] [--generates-media] [--no-high-risk-actions] [--json]`. Run `audit`, `scan` and `c2pa` first, and keep only the answers that evidence shows as live.

Without Node 20+, the same image check is one curl call:

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
| [`eu-ai-act-rfp-responder`](skills/eu-ai-act-rfp-responder/SKILL.md) | *"Answer this customer's EU AI Act security questionnaire."* | Runs `audit`, `scan` and `c2pa` on the repo and live site, writes an evidence ledger, then drafts the five standard procurement answers (Art. 50(1) and 50(2), Art. 12 logging, GDPR Art. 5(1)(c) and Frankfurt residency, Art. 14 oversight, retention and Annex IV evidence), marking each control live, partial or not yet in place. The fast path is MCP tool #10 or `npx -y spurvik@1.0.15 rfp`, checked against that ledger. |

No account or API key needed for the local setup.

## Just want the notice? One line

```html
<script src="https://spurvik.com/badge.js" data-system="Support Assistant" defer></script>
```

No cookies, no network telemetry, 24 EU languages, Shadow DOM. Works in plain HTML, Shopify, WordPress, Webflow, Lovable and Bolt.

`data-attribution` defaults to `true`, which shows a small "Art. 50 · Spurvik" trust link after the notice (Free). Set `data-attribution="false"` to hide it for white-label on Pro (€19/mo) or Done-For-You (€199).

- **Shopify (Online Store 2.0 section, no app):** [spurvik-shopify-art50.liquid](https://spurvik.com/r/spurvik-shopify-art50.liquid). Save it as `sections/spurvik-art50-notice.liquid`, then add the "Spurvik Art. 50 notice" section to the footer. Language Auto maps `request.locale.iso_code` to the 24 EU languages; the position defaults to `bottom-left` so chat launchers on the right stay clear.
- **WordPress plugin v1.1.0 (no code):** [spurvik-eu-ai-act-wp.zip](https://spurvik.com/r/spurvik-eu-ai-act-wp.zip), via Plugins, Add New, Upload Plugin. Add `[spurvik_status_shield]` to a footer widget or Shortcode block for a live status shield.
- **PrestaShop 1.7 / 8 / 9 module (no code):** [spurvik-prestashop-art50.zip](https://spurvik.com/r/spurvik-prestashop-art50.zip), uploaded via Modules, Module Manager, Upload a module.
- **Shopware 6.5 / 6.6+ extension (no code):** [spurvik-shopware6-art50.zip](https://spurvik.com/r/spurvik-shopware6-art50.zip), uploaded via Extensions, My extensions, Upload extension.
- **Cloudflare Edge Worker (no origin edits):** [cloudflare-worker-art50-c2pa.js](https://spurvik.com/r/cloudflare-worker-art50-c2pa.js) (`/.well-known/ai-act.json` edge proxy + `HTMLRewriter` Art. 50(1) notice + C2PA `polish: "off"` passthrough; interactive `/trust` & manifest studio at [spurvik.com/well-known-ai-act-json-trust-center](https://spurvik.com/well-known-ai-act-json-trust-center)).
- **Google Tag Manager container (no code):** [spurvik-gtm-art50-container.json](https://spurvik.com/r/spurvik-gtm-art50-container.json), imported via GTM Admin, Import Container, Merge (`DOM Ready`, 4 editable variables).
- **Framer component:** [spurvik-framer-badge.tsx](https://spurvik.com/r/spurvik-framer-badge.tsx), pasted into Assets, Code, New Code File.
- **shadcn / v0 registry:** `npx shadcn@latest add https://spurvik.com/r/article-50-badge.json`

## SDK

```bash
npm i spurvik
```

`withSpurvik(client, config)` wraps OpenAI, Anthropic and Gemini clients. `spurvikMiddleware` from `spurvik/ai` plugs into the Vercel AI SDK, with adapters for Mastra, OpenAI Agents and LangGraph. It records each call with a keyed HMAC receipt; the prompt and output text are not sent. MIT, source at [spurvik/spurvik](https://github.com/spurvik/spurvik).

For **Python 3.10+ (FastAPI, LangChain, PydanticAI, Django, OpenAI Python SDK)** with zero extra pip dependencies: `curl -fsSLO https://spurvik.com/r/spurvik_guard.py` (includes `verify_signed_callback` + offline 6-vector Art. 14 blocking self-test `python3 spurvik_guard.py [domain] [--json]`; [interactive Python cockpit](https://spurvik.com/python-fastapi-langchain-eu-ai-act-logging)).

## MCP server

Listed in the official MCP Registry as `io.github.spurvik/spurvik-mcp-server` (and `com.spurvik/eu-ai-act`). For OpenAI Custom GPT Actions, Microsoft Copilot Studio, Dust, Dify, Zapier or Make, import `https://console.spurvik.com/openapi.json` (20 paths, including `GET https://console.spurvik.com/api/v1/portfolio/scan?domains=client1.com,client2.com` 1-to-10 domain batch scanner, `GET https://console.spurvik.com/api/v1/report/markdown?domain=yourdomain.com&format=raw` 1-page client/DPO Markdown brief with live Art. 50 + C2PA SVG shields, `GET https://console.spurvik.com/api/v1/badge/card.svg?domain=yourdomain.com` 10-language 460x148 SaaS Trust Center / Notion SVG readiness card, `GET https://console.spurvik.com/api/v1/badge/ai-act.json?domain=yourdomain.com` machine-readable `/.well-known/ai-act.json` generator (`spurvik.ai-act-manifest.v1`), `GET https://console.spurvik.com/api/v1/c2pa/check?url=yourdomain.com` 1-hop C2PA & CDN-strip inspector, `GET https://console.spurvik.com/api/v1/gate/self-test?domain=yourdomain.com` 6-vector Art. 14 gate benchmark, `GET https://console.spurvik.com/api/v1/rfp/answers?domain=yourdomain.com&plan=pro` 8-language vendor RFP & DPO questionnaire generator, `GET https://console.spurvik.com/api/v1/risk/classify?domain=yourdomain.com&preset=support-chatbot` 10-language risk-tier & DPO Self-Assessment Markdown API, `GET https://console.spurvik.com/api/v1/badge/snippet?domain=yourdomain.com&notice=interaction` 24-language Art. 50 `badge.js` / React / CMS snippet API, `GET https://console.spurvik.com/api/v1/setup/snippet?target=fastapi&domain=yourdomain.com` 40+ stack SDK/Python/CMS setup API, `POST https://console.spurvik.com/api/v1/audit/unwrapped` zero-echo TS/JS/Python unwrapped AI call scanner, and `POST https://console.spurvik.com/api/v1/receipt/verify` Art. 12 HMAC receipt & Art. 14 signed approval callback inspector; A2A Agent Card: `https://console.spurvik.com/.well-known/agent.json`). The hosted server at `https://console.spurvik.com/api/mcp` and the local stdio server `spurvik-mcp-server@1.0.12` expose the same 10 zero-key tools:

| Tool | What it does |
| --- | --- |
| `spurvik_scan_live_url` | Scans one public HTTPS page for AI chat widgets and SDKs, checks for a visible Art. 50(1) notice and returns a one-line fix |
| `spurvik_batch_scan_domains` | Scans 1 to 10 public domains in parallel (for example an agency's client sites). Returns the widget and SDK gaps, a `badge.js` fix in each site's language (24 EU languages), a shareable `/report?domain=` brief URL and a live status shield in Markdown per domain |
| `spurvik_classify_ai_act_risk` | Deterministic EU AI Act risk-tier mapping (`prohibited_art5`, `high_risk_annex_iii`, `limited_transparency_art50`, `minimal_risk`) with optional `siteDomain` + `locale` prefill and `selfAssessmentMarkdown`. You still decide your tier |
| `spurvik_generate_art50_badge` | Copy-paste Art. 50 notice (badge.js tag or React snippet) in any of the 24 EU languages |
| `spurvik_setup_snippet` | Setup snippets for OpenAI, Anthropic, Gemini (`google-genai`), Vercel AI SDK (`nextjs`/`vercel`), Lovable + Supabase (`lovable`/`supabase`), n8n & Make (`n8n`/`make`), Mastra, OpenAI Agents, LangGraph, React, Python, FastAPI, LangChain, PydanticAI, Django (`spurvik_guard.py`), Intercom Fin, Zendesk AI, Tidio Lyro, HubSpot Breeze, Dixa, Trengo, Shopify (`.liquid`), WordPress/WooCommerce (`wp_footer` / `.zip`), Google Tag Manager (`gtm` / `.json`), Webflow, Shopware & HTML (`badge.js`) |
| `spurvik_find_unwrapped_calls` | Heuristic scan of pasted TypeScript/JS or Python code (or a workspace directory — `package.json`, `pyproject.toml`, `requirements.txt` — on stdio) for AI calls not wrapped with Spurvik |
| `spurvik_explain_receipt` | Explains (and optionally verifies) an HMAC call receipt |
| `spurvik_marking_hooks` | How to plug your own Art. 50(2) marking into the SDK |
| `spurvik_check_image_c2pa_url` | Checks one public https image URL (PNG, JPEG or WebP, up to 4 MB) — or a bare public domain in 1 hop — for an embedded C2PA manifest, its signature and trust list result, and image optimizers that strip it (`/_next/image`, Shopify CDN, Jetpack Photon, Webflow CDN, Cloudinary, Imgix, Cloudflare). Read-only, nothing stored, 10 checks a minute |
| `spurvik_vendor_questionnaire_answers` | New in 1.0.8, on hosted and local. Copy-paste Markdown answers to the five EU AI Act questions in enterprise vendor security questionnaires and DPO RFPs (Q1–Q5: Art. 50(1)/(2), Art. 12 logging, GDPR Art. 5(1)(c) minimisation and Frankfurt residency, Art. 14 approval gates, retention and Annex IV), matched to the Free, Pro, Team or Done-For-You plan. Deterministic, makes no outbound network calls, every input optional. Check each answer against your own setup before you send it. The same draft works offline with `npx -y spurvik@1.0.15 rfp` |

```bash
# Remote (Streamable HTTP), 10 tools
claude mcp add --transport http spurvik https://console.spurvik.com/api/mcp

# Local stdio, the same 10 tools; spurvik_find_unwrapped_calls can also scan a workspace directory
npx -y spurvik-mcp-server@1.0.12
```

## Try it

- European Language Portals (14 native guides + live Art. 50 scanner each): [EN](https://spurvik.com/) · [DE](https://spurvik.com/de) · [FR](https://spurvik.com/fr) · [NL](https://spurvik.com/nl) · [DA](https://spurvik.com/da) · [SV](https://spurvik.com/sv) · [ES](https://spurvik.com/es) · [IT](https://spurvik.com/it)
- Sandbox: https://console.spurvik.com/sandbox
- Free EU AI Act risk check: https://console.spurvik.com/risk-check
- Free Art. 50(2) C2PA manifest and CDN-strip checker: https://console.spurvik.com/c2pa (interactive 2 Dec 2026 scope & C2PA cockpits in [EN](https://spurvik.com/eu-ai-act-2-december-2026-c2pa-provenance), [DE](https://spurvik.com/de/ki-verordnung-2-dezember-2026-artikel-50-c2pa-herkunftsnachweis), [FR](https://spurvik.com/fr/reglement-ia-2-decembre-2026-article-50-provenance-c2pa), [NL](https://spurvik.com/nl/ai-verordening-2-december-2026-artikel-50-c2pa-herkomst), [DA](https://spurvik.com/da/eu-ai-act-2-december-2026-artikel-50-c2pa-oprindelse), [SV](https://spurvik.com/sv/ai-forordningen-2-december-2026-artikel-50-c2pa-ursprung), [ES](https://spurvik.com/es/reglamento-ia-2-diciembre-2026-articulo-50-procedencia-c2pa), [IT](https://spurvik.com/it/regolamento-ia-2-dicembre-2026-articolo-50-provenienza-c2pa))
- Free agency batch scanner (up to 10 domains): https://console.spurvik.com/portfolio (interactive agency retainer & margin simulator in [EN](https://spurvik.com/eu-ai-act-for-agencies), [DE](https://spurvik.com/de/ki-verordnung-fuer-agenturen-artikel-50-white-label), [FR](https://spurvik.com/fr/reglement-ia-pour-agences-audit-clients-article-50), [NL](https://spurvik.com/nl/ai-verordening-voor-bureaus-klantenaudit-artikel-50-white-label), [DA](https://spurvik.com/da/eu-ai-act-for-bureauer-kundetjek-artikel-50-white-label), [SV](https://spurvik.com/sv/ai-forordningen-for-byraer-kundrevision-artikel-50-white-label), [ES](https://spurvik.com/es/reglamento-ia-para-agencias-auditoria-clientes-articulo-50-marca-blanca), [IT](https://spurvik.com/it/regolamento-ia-per-agenzie-audit-clienti-articolo-50-white-label))
- EU AI Act vendor security questionnaire answers (5 questions, `.md` export): [EN](https://spurvik.com/eu-ai-act-vendor-security-questionnaire) · [DE](https://spurvik.com/de/ki-verordnung-sicherheitsfragebogen-lieferanten-dsb) · [FR](https://spurvik.com/fr/questionnaire-securite-reglement-ia-dpo-fournisseur) · [NL](https://spurvik.com/nl/ai-verordening-leveranciersvragenlijst-fg-beveiliging) · [DA](https://spurvik.com/da/eu-ai-act-leverandoerspoergeskema-dpo-sikkerhed) · [SV](https://spurvik.com/sv/ai-forordningen-leverantorssakerhetsformular-dso) · [ES](https://spurvik.com/es/cuestionario-seguridad-proveedor-reglamento-ia-dpd) · [IT](https://spurvik.com/it/questionario-sicurezza-fornitori-regolamento-ia-dpo)
- Self-hosted n8n & Make.com EU AI Act workflow guard (`n8n-nodes-spurvik@1.0.12`, 7 operations): [EN](https://spurvik.com/n8n-eu-ai-act-guard) · [DE](https://spurvik.com/de/n8n-make-ki-verordnung-artikel-50-hmac-freigabe) · [FR](https://spurvik.com/fr/n8n-make-reglement-ia-article-50-hmac-approbation) · [NL](https://spurvik.com/nl/n8n-make-ai-verordening-artikel-50-hmac-goedkeuring) · [DA](https://spurvik.com/da/n8n-make-eu-ai-act-artikel-50-hmac-godkendelse) · [SV](https://spurvik.com/sv/n8n-make-ai-forordningen-artikel-50-hmac-godkannande) · [ES](https://spurvik.com/es/n8n-make-reglamento-ia-articulo-50-hmac-aprobacion) · [IT](https://spurvik.com/it/n8n-make-regolamento-ia-articolo-50-hmac-approvazione)
- Lovable, Bolt.new, v0 & Replit EU AI Act prompt cockpit (Supabase Edge Functions + `badge.js`): [EN](https://spurvik.com/lovable-bolt-replit-eu-ai-act) · [DE](https://spurvik.com/de/lovable-bolt-replit-ki-verordnung-artikel-50) · [FR](https://spurvik.com/fr/lovable-bolt-replit-reglement-ia-article-50) · [NL](https://spurvik.com/nl/lovable-bolt-replit-ai-verordening-artikel-50) · [DA](https://spurvik.com/da/lovable-bolt-replit-eu-ai-act-artikel-50) · [SV](https://spurvik.com/sv/lovable-bolt-replit-ai-forordningen-artikel-50) · [ES](https://spurvik.com/es/lovable-bolt-replit-reglamento-ia-articulo-50) · [IT](https://spurvik.com/it/lovable-bolt-replit-regolamento-ia-articolo-50)
- Python, FastAPI, LangChain, PydanticAI & Django EU AI Act cockpit (`spurvik_guard.py`, 0 extra pip packages): [EN](https://spurvik.com/python-fastapi-langchain-eu-ai-act-logging) · [DE](https://spurvik.com/de/python-fastapi-langchain-ki-verordnung-logging) · [FR](https://spurvik.com/fr/python-fastapi-langchain-reglement-ia-journalisation) · [NL](https://spurvik.com/nl/python-fastapi-langchain-ai-verordening-logging) · [DA](https://spurvik.com/da/python-fastapi-langchain-eu-ai-act-logning) · [SV](https://spurvik.com/sv/python-fastapi-langchain-ai-forordningen-loggning) · [ES](https://spurvik.com/es/python-fastapi-langchain-reglamento-ia-registro) · [IT](https://spurvik.com/it/python-fastapi-langchain-regolamento-ia-logging)
- Intercom Fin, Zendesk AI, Tidio Lyro, HubSpot Breeze, Dixa & Trengo Art. 50(1) support-widget cockpit (`badge.js`, 0 cookies): [EN](https://spurvik.com/intercom-zendesk-tidio-hubspot-eu-ai-act-disclosure) · [DE](https://spurvik.com/de/intercom-fin-zendesk-tidio-hubspot-ki-verordnung-artikel-50) · [FR](https://spurvik.com/fr/intercom-fin-zendesk-tidio-hubspot-reglement-ia-article-50) · [NL](https://spurvik.com/nl/intercom-fin-zendesk-trengo-tidio-ai-verordening-artikel-50) · [DA](https://spurvik.com/da/intercom-fin-zendesk-dixa-tidio-eu-ai-act-artikel-50) · [SV](https://spurvik.com/sv/intercom-fin-zendesk-tidio-hubspot-ai-forordningen-artikel-50) · [ES](https://spurvik.com/es/intercom-fin-zendesk-tidio-hubspot-reglamento-ia-articulo-50) · [IT](https://spurvik.com/it/intercom-fin-zendesk-tidio-hubspot-regolamento-ia-articolo-50)
- Cursor, Claude Code, VS Code MCP & GitHub Actions PR-Gate cockpit (`spurvik-mcp-server@1.0.12`, 10 zero-key tools): [EN](https://spurvik.com/cursor-claude-mcp-eu-ai-act) · [DE](https://spurvik.com/de/cursor-claude-mcp-github-actions-ki-verordnung) · [FR](https://spurvik.com/fr/cursor-claude-mcp-github-actions-reglement-ia) · [NL](https://spurvik.com/nl/cursor-claude-mcp-github-actions-ai-verordening) · [DA](https://spurvik.com/da/cursor-claude-mcp-github-actions-eu-ai-act) · [SV](https://spurvik.com/sv/cursor-claude-mcp-github-actions-ai-forordningen) · [ES](https://spurvik.com/es/cursor-claude-mcp-github-actions-reglamento-ia) · [IT](https://spurvik.com/it/cursor-claude-mcp-github-actions-regolamento-ia)
- WordPress (`v1.1.0` `.zip` + `[spurvik_status_shield]`), Elementor, Webflow & Framer Art. 50(1) cockpit (`badge.js`, 0 cookies): [EN](https://spurvik.com/wordpress-shopify-webflow-eu-ai-act-badge) · [DE](https://spurvik.com/de/wordpress-elementor-webflow-ki-verordnung-plugin-artikel-50) · [FR](https://spurvik.com/fr/wordpress-elementor-webflow-plugin-reglement-ia-article-50) · [NL](https://spurvik.com/nl/wordpress-elementor-webflow-ai-verordening-plugin-artikel-50) · [DA](https://spurvik.com/da/wordpress-elementor-webflow-eu-ai-act-plugin-artikel-50) · [SV](https://spurvik.com/sv/wordpress-elementor-webflow-ai-forordningen-plugin-artikel-50) · [ES](https://spurvik.com/es/wordpress-elementor-webflow-plugin-reglamento-ia-articulo-50) · [IT](https://spurvik.com/it/wordpress-elementor-webflow-plugin-regolamento-ia-articolo-50)
- EU AI Act Art. 12 & GDPR Art. 5(1)(c) zero-prompt-text HMAC-SHA256 receipt & Annex IV cockpit (`spurvik@1.0.15` + `spurvik_guard.py`, live WebCrypto demo): [EN](https://spurvik.com/ai-event-logging-hmac-receipts) · [DE](https://spurvik.com/de/ki-verordnung-artikel-12-protokollierung-dsgvo-hmac) · [FR](https://spurvik.com/fr/reglement-ia-article-12-journalisation-rgpd-hmac) · [NL](https://spurvik.com/nl/ai-verordening-artikel-12-logging-avg-hmac) · [DA](https://spurvik.com/da/eu-ai-act-artikel-12-logning-gdpr-hmac) · [SV](https://spurvik.com/sv/ai-forordningen-artikel-12-loggning-gdpr-hmac) · [ES](https://spurvik.com/es/reglamento-ia-articulo-12-registro-rgpd-hmac) · [IT](https://spurvik.com/it/regolamento-ia-articolo-12-registrazione-gdpr-hmac)
- EU AI Act Art. 14 human-in-the-loop pre-execution approval gate cockpit (`Slack`, `Microsoft Teams`, `LangGraph spurvikInterrupt`, `OpenAI Agents createSpurvikNeedsApproval` & `spurvik_guard.py`): [EN](https://spurvik.com/langgraph-openai-agents-approval-check) · [DE](https://spurvik.com/de/ki-verordnung-artikel-14-menschliche-aufsicht-slack-teams-freigabe) · [FR](https://spurvik.com/fr/reglement-ia-article-14-controle-humain-validation-slack-teams) · [NL](https://spurvik.com/nl/ai-verordening-artikel-14-menselijk-toezicht-slack-teams-goedkeuring) · [DA](https://spurvik.com/da/eu-ai-act-artikel-14-menneskeligt-tilsyn-slack-teams-godkendelse) · [SV](https://spurvik.com/sv/ai-forordningen-artikel-14-mansklig-tillsyn-slack-teams-godkannande) · [ES](https://spurvik.com/es/reglamento-ia-articulo-14-supervision-humana-aprobacion-slack-teams) · [IT](https://spurvik.com/it/regolamento-ia-articolo-14-sorveglianza-umana-approvazione-slack-teams)
- Next.js App Router, React `<AIDisclosureLabel />`, `shadcn` / `v0` Registry & Vercel AI SDK (`spurvikMiddleware`) Art. 50(1) + HMAC cockpit (`spurvik@1.0.15`, live `badge.js` preview & README status shield): [EN](https://spurvik.com/nextjs-vercel-ai-sdk-art50-badge) · [DE](https://spurvik.com/de/nextjs-vercel-ai-sdk-ki-verordnung-artikel-50) · [FR](https://spurvik.com/fr/nextjs-vercel-ai-sdk-reglement-ia-article-50) · [NL](https://spurvik.com/nl/nextjs-vercel-ai-sdk-ai-verordening-artikel-50) · [DA](https://spurvik.com/da/nextjs-vercel-ai-sdk-eu-ai-act-artikel-50) · [SV](https://spurvik.com/sv/nextjs-vercel-ai-sdk-ai-forordningen-artikel-50) · [ES](https://spurvik.com/es/nextjs-vercel-ai-sdk-reglamento-ia-articulo-50) · [IT](https://spurvik.com/it/nextjs-vercel-ai-sdk-regolamento-ia-articolo-50)
- Workspace (Frankfurt, fra1): https://console.spurvik.com/signup
- Plans: https://spurvik.com/#pricing

| Plan | Price | Adds |
| --- | --- | --- |
| Free | €0 | 24-language notice badge, SDK, 10-tool MCP server, 10k events/mo (30d Frankfurt retention) |
| Pro | €19/mo (€190/yr) | 250k events/mo, 90-day Frankfurt (`fra1`) log retention, hosted human approval queue, DPO engineering summary `.md` download, white-label badge (`data-attribution="false"`) |
| Team | €79/mo (€790/yr) | Everything in Pro, 2,000,000 events/mo, 12-month Frankfurt (`fra1`) log retention, webhook export, Annex IV technical documentation draft |
| Done-For-You 24h Setup | €199 once | We wire it into your app within 24 hours. Includes 1 full year of Pro (€190 value). [Buy now](https://console.spurvik.com/signup?plan=concierge) |

Prices excl. VAT, B2B.

## License

MIT © EyeTea Solutions ApS

<sub>Scope: Spurvik supports Art. 50(1) notices and, for high-risk systems, Art. 12 logging and Art. 14 oversight. It does not classify your use case, sign or mark content for Art. 50(2), or make your system meet the EU AI Act. Not legal advice.</sub>
