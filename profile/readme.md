# Swatchdog Antigravity Bridge (Beta)

<!-- Add image_3.png here as a clean header -->
![Swatchdog Banner](swatchdog-github-banner.svg)

# Swatchdog MCP Server

> Give your AI agents a design-system standard to build from, so they use your real
> color, radius, spacing, and type tokens instead of guessing.

**Swatchdog** is a Model Context Protocol (MCP) server that runs **on-demand** CSS
design-token drift checks for AI coding assistants (Google Antigravity, Claude Code,
Cursor, and other MCP-aware clients).

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
4. The agent applies the fixes on a new Git branch for your review.

---

## Core Features

### 1. Pack Mode (Preset Validation)
Check your styles against a curated Swatchdog design family — **Workbench, Showcase,
Terminal**, plus the free **Studio** sandbox. Fast, zero-configuration checks.

### 2. BYO Mode (Bring Your Own Tokens) *New in v3*
Check styles against your project's own custom design system.
* The agent extracts tokens locally from files like `tailwind.config.js` or your CSS
  variables and passes them as parameters.
* Content-only and stateless — your source code and custom tokens are never stored.

### 3. Intelligent Drift Suggestions
Swatchdog doesn't just flag drift; it maps each off-token value to the closest valid token:
* **Color**: off-palette hex/rgb → the nearest theme token.
* **Radius**: off-scale border-radii → the nearest radius token.
* **Spacing & Typography**: off-scale padding/margin, font-size, and font-family.

*Note: validation currently covers hex and standard color formats. HSL-channel
representation and complex multi-file token resolution are planned for Phase 2.*

---
## Supported tool

check_drift — checks a CSS string against a pack or a custom token set.

content (string, required) — the CSS/markup to check.
paletteId (string, optional) — a pack id (e.g. studio-blue-hour). Pack mode.
tokens (object, optional) — your own token set. BYO mode (paid key).
source (string, optional) — telemetry tag (pack, css, tailwind).

## Sandbox & gating

Free sandbox key (swt_sandbox_studio) — check CSS against the free Studio family.
Paid key — premium families (Workbench, Showcase, Terminal) and BYO checks.
If a free client attempts a premium or BYO check, the tool returns a structured JSON
upgrade payload; the agent surfaces a link to buy a single pack ($19) or the all-in
bundle ($49).

## Privacy & telemetry
All checks are on-demand and transient.

No source code, files, or custom tokens are ever stored on our servers.
We log minimal usage metadata — the source tag, which pack, and the finding
outcome, tied to the account the key belongs to — never your license key, your
source code, or your tokens.

Created and maintained by swatchdog.dev.

---

## Client Integration

Add Swatchdog to your agent's MCP config:

```json
{
  "mcpServers": {
    "swatchdog": {
      "type": "http",
      "url": "https://swatchdog-mcp-970396648818.us-central1.run.app/mcp",
      "headers": { "Authorization": "Bearer swt_sandbox_studio" }
    }

  }
}



