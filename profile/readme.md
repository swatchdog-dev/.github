
![Swatchdog Banner](swatchdog-github-banner.svg)

# Swatchdog

> Give your AI agent a visual system to build against — so it writes brand-consistent styles instead of guessing colors, radii, spacing, and type.

**✅ Now in the [Anthropic Connector Directory](https://claude.ai/directory/connectors/swatchdog).** In Claude, open **Settings → Connectors**, search **swatchdog**, and click **Connect** — no URL, no key, no setup.

---

## See it in action

- **Live claude.ai run:** [connector session walkthrough →](https://github.com/swatchdog-dev/swatchdog-mcp/blob/main/docs/demo/live_session.md)
- **Full test transcript (all green):** [part-b-transcript.txt →](https://github.com/swatchdog-dev/swatchdog-mcp/blob/main/docs/demo/part-b-transcript.txt)

https://github.com/user-attachments/assets/6ae8b242-d6c7-4a29-8892-27b067835308

---

## What it is

**Swatchdog** is a Model Context Protocol (MCP) server that runs **on-demand** design-token drift checks for AI coding assistants — Claude Code, Claude Desktop, Cursor, Google Antigravity, other MCP clients, and **claude.ai** via the connector.

When your agent asks, Swatchdog checks the generated CSS against a design system — a curated pack or your own tokens — and returns every off-token color, radius, spacing, and type value, each with the closest matching token to use instead. The check is on-demand and the loop is yours: **Swatchdog reports drift; it never intercepts writes or changes your code.**

---

## Why Swatchdog

AI agents are fast but blind to your design system — they invent off-brand spacing, off-palette colors, and quietly ignore your scales. Swatchdog adds a deterministic drift check to the build loop:

1. The agent sets a standard — your own tokens, or a pack.
2. It calls Swatchdog on the CSS it generated.
3. Swatchdog returns precise fixes (e.g. *"use `radius.sm` (5px) instead of 7px"*).
4. The agent applies them. The loop, and the review, stay yours.

**How is it different from a linter like ESLint?**
A linter checks whether your code is *valid*; Swatchdog checks whether your design is *on-system*. A linter happily passes `color: #ff00ff` because it's valid CSS — it has no idea magenta isn't in your palette. Swatchdog catches exactly that, even when the code is flawless. Lint catches broken code; Swatchdog catches broken design.

**Can't I just give my agent the tokens, or a `design.md`?**
You can, and it'll still drift. Agents approximate and interpolate even with the rules right in front of them — having the rules isn't the same as following them. Swatchdog verifies conformance in the build loop, where the drift actually happens.

---

## Modes

**Pack mode** — check against a curated Swatchdog family (**Workbench, Showcase, Terminal**, plus the free **Studio** sandbox). Zero-config.

**BYO mode** — check against your *own* design system. The agent extracts your tokens (from `tailwind.config.js`, CSS variables, etc.) and passes them as parameters. Content-only and stateless — your code and tokens are never stored. Free to try on the keyless connector lane, or uncapped with a $12 license.

**Intelligent suggestions** — Swatchdog doesn't just flag drift, it maps each off-token value to the nearest valid token: color (hex/rgb → nearest theme color), radius, spacing, font-size, and font-family.

> *Coverage note: checks currently cover hex and standard color formats. HSL-channel representation and complex multi-file token resolution are on the Phase 2 roadmap.*

---

## Tools

**`check_design_drift`** — connector endpoint · BYO-only · keyless-friendly

- `reference_tokens` *(object, required)* — your design tokens, e.g. `{"color":{"primary":"#b06ed0"},"radius":{"md":"6px"}}`
- `code` *(string, required)* — the CSS/markup to check
- Returns, per violation: axis · found value · expected token + value · location

**`check_drift`** — main endpoint · checks CSS against a pack or a custom token set

- `content` *(string, required)* — the CSS/markup to check
- `paletteId` *(string, optional)* — a pack id (e.g. `studio-blue-hour`); pack mode
- `tokens` *(object, optional)* — your own token set; BYO mode
- `source` *(string, optional)* — telemetry tag (`pack`, `css`, `tailwind`)

---

## Pricing — one-time, no subscriptions

| Tier | What you get | Where |
|---|---|---|
| **Free** — keyless | BYO checks on a shared, rate-capped lane | connector endpoint |
| **Free** — sandbox key `swt_sandbox_studio` | pack checks vs the **Studio** family | main endpoint |
| **$12** — drift-check license | BYO checks, your own uncapped key | both endpoints |
| **$19 / $49** — pack or bundle | premium families (Workbench · Showcase · Terminal) **+** a paid key | both endpoints |

On the main endpoint, a free caller attempting a premium or BYO check gets a structured upgrade payload pointing to [swatchdog.dev](https://swatchdog.dev).

---

## Connect

**In Claude (easiest):** Settings → Connectors → find **swatchdog** in the directory → **Connect**. Nothing to paste, no key.

**Other MCP clients** (Cursor, Claude Code, Claude Desktop) — add one of these to your MCP config:

Connector endpoint — BYO-only, keyless (add a key to remove the rate cap):

```json
{
  "mcpServers": {
    "swatchdog-check": {
      "type": "http",
      "url": "https://swatchdog-connector-970396648818.us-central1.run.app/mcp"
    }
  }
}
```

Main endpoint — packs + BYO, bearer key (free sandbox key shown):

```json
{
  "mcpServers": {
    "swatchdog-sandbox": {
      "type": "http",
      "url": "https://swatchdog-mcp-970396648818.us-central1.run.app/mcp",
      "headers": { "Authorization": "Bearer swt_sandbox_studio" }
    }
  }
}
```

*Prefer to add the connector to claude.ai manually? Settings → Connectors → Add custom connector → paste the connector URL above → leave auth empty.*

---

## Privacy

All checks are on-demand and transient. **No source code, files, or tokens are ever stored on our servers.** We log only minimal usage metadata — a source tag, which pack, and the finding count — never your license key, your code, or your tokens. Full policy: [swatchdog.dev/privacy.html](https://swatchdog.dev/privacy.html).

---

Created and maintained by [swatchdog.dev](https://swatchdog.dev) · Support: hey@swatchdog.dev · [A Ziola Project](https://www.ziola.dev/index.html)
