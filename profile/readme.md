
![Swatchdog Banner](swatchdog-github-banner.svg)

# Swatchdog MCP Server 

> Give your AI agents a visual system guide so they write brand-consistent styles instead of guessing color values, radii, and layouts.

---

### See Swatchdog in Action

**Tested live:** [claude.ai connector run →](https://github.com/swatchdog-dev/swatchdog-mcp/blob/main/docs/demo/live_session.md)
**Full test transcript, all green:** [part-b-transcript.txt](https://github.com/swatchdog-dev/swatchdog-mcp/blob/main/docs/demo/part-b-transcript.txt)

https://github.com/user-attachments/assets/6ae8b242-d6c7-4a29-8892-27b067835308

# Swatchdog

**Swatchdog** is a Model Context Protocol (MCP) server that runs **on-demand** CSS
design-token drift checks for AI coding assistants (Claude Code, Claude Desktop, Cursor,
Google Antigravity, and other MCP-aware clients — plus claude.ai via a connector).

When your agent asks, Swatchdog checks the generated CSS against a chosen design pack —
or your own tokens — and returns the off-token color, radius, spacing, and type values
with the closest matching token to use instead. The check is on-demand and the loop is
yours: **Swatchdog reports drift; it does not intercept writes or change your code.**

---

## Why Swatchdog?

AI coding agents are fast, but blind to your design system — they invent off-brand
spacing, off-palette colors, and ignore your scales.

Swatchdog adds an **on-demand drift check** to the agentic loop. When connected:
1. The agent detects your design tokens (or you pick a pack).
2. The agent calls Swatchdog to check the CSS it generated.
3. Swatchdog returns precise suggestions (e.g., *"Use `radius.sm` (5px) instead of 7px"*).
4. The agent applies the fixes. The loop, and the review, stay yours.

### How is swatchdog different from a linter like ESLint?
A linter checks whether your code is valid and well-formatted. swatchdog checks whether your design is on-system. A linter happily passes color: #ff00ff because it's valid CSS — it has no idea magenta isn't in your palette. swatchdog catches exactly that: colors, spacing, radii, and type that have drifted from your reference, even when the code is flawless. Lint catches broken code; swatchdog catches broken design.

### Can't I just give my AI agent the design tokens or a design.md?
You can, and it'll still drift. Agents approximate and interpolate even with the rules right in front of them — a tokens file or a design.md is instruction, and having the rules isn't the same as following them. swatchdog verifies conformance in the build loop, where the drift actually happens.
>

---

## Core Features

### 1. Pack Mode (Preset Checks)
Check your styles against a curated Swatchdog design family — **Workbench, Showcase,
Terminal**, plus the free **Studio** sandbox. Fast, zero-configuration checks.

### 2. BYO Mode (Bring Your Own Tokens)
Check styles against your project's own custom design system.
* The agent extracts tokens locally from files like `tailwind.config.js` or your CSS
  variables and passes them as parameters.
* Content-only and stateless — your source code and custom tokens are never stored.
* **Free to try** on the connector endpoint (no key — shared, rate-capped lane), or
  uncapped with any paid key, including the standalone **$12 drift-check license**.

### 3. Connector Endpoint (keyless) — *new*
A dedicated, trimmed BYO-only surface built for chat clients:
* One read-only tool — `check_design_drift(reference_tokens, code)` — returning a
  compact, machine-first report sized for an agent's fix-and-recheck loop.
* **No key required**: keyless calls ride a shared free lane (best-effort, rate-capped
  with a clean `429 + Retry-After` at capacity). A paid key in the `Authorization`
  header gets its own uncapped lane.
* In **claude.ai**: add it as a custom connector (Settings → Connectors → Add custom
  connector, URL below, no credentials).

### 4. Intelligent Drift Suggestions
Swatchdog doesn't just flag drift; it maps each off-token value to the closest valid token:
* **Color**: off-palette hex/rgb → the nearest theme token.
* **Radius**: off-scale border-radii → the nearest radius token.
* **Spacing & Typography**: off-scale padding/margin, font-size, and font-family.

*Note: checks currently cover hex and standard color formats. HSL-channel
representation and complex multi-file token resolution are planned for Phase 2.*

---

## Supported tools

**`check_drift`** — main endpoint. Checks a CSS string against a pack or a custom token set.

* `content` (string, required) — the CSS/markup to check.
* `paletteId` (string, optional) — a pack id (e.g. `studio-blue-hour`). Pack mode.
* `tokens` (object, optional) — your own token set. BYO mode (any paid key).
* `source` (string, optional) — telemetry tag (`pack`, `css`, `tailwind`).

**`check_design_drift`** — connector endpoint. BYO-only, keyless-friendly.

* `reference_tokens` (object, required) — your design tokens
  (e.g. `{"color":{"primary":"#b06ed0"},"radius":{"md":"6px"}}`).
* `code` (string, required) — the CSS/markup to check.
* Returns per-violation: axis · found value · expected token + value · location.

## Access & pricing (one-time, no subscriptions)

| Lane | What you get | Where |
|---|---|---|
| **Free** — sandbox key `swt_sandbox_studio` | pack checks vs the **Studio** family | main endpoint |
| **Free** — keyless | BYO checks, shared rate-capped lane | connector endpoint |
| **$12** — drift-check license | BYO checks, your own uncapped key | both endpoints (key sent as a bearer header) |
| **$19 / $49** — pack or bundle | premium families (Workbench, Showcase, Terminal) **+** a paid key (BYO included) | both endpoints |

If a free caller attempts a premium or BYO check on the main endpoint, the tool returns
a structured JSON upgrade payload with where to get a key — at
[swatchdog.dev](https://swatchdog.dev).

## Privacy & telemetry

All checks are on-demand and transient.

No source code, files, or custom tokens are ever stored on our servers.
We log minimal usage metadata — the source tag, which pack, and the finding
outcome, tied to the account the key belongs to — never your license key, your
source code, or your tokens.

Created and maintained by [swatchdog.dev](https://swatchdog.dev) ·
privacy policy: [swatchdog.dev/privacy.html](https://swatchdog.dev/privacy.html) ·
support: hey@swatchdog.dev

---

## Client Integration

**Main endpoint** (packs + BYO, bearer key — the free sandbox key shown):

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

**Connector endpoint** ( BYO-only, no key needed — add a key to remove the rate cap):

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

In **claude.ai**: Settings → Connectors → Add custom connector → paste the connector
URL above — no credentials needed.
