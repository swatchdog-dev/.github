# Swatchdog Antigravity Bridge (Beta)

<!-- Add image_3.png here as a clean header -->
<img src="https://github.com/swatchdog-dev/swatchdog-antigravity-bridge/raw/main/swatchdog-banner.png" width="100%" alt="Swatchdog Agentic Validator Banner">

**The World's First Agentic Design Token Validator**

Autonomous agents (like Gemini 3.5 Flash in Antigravity 2.0) are amazing at code, but terrible at consistent visual style. `Swatchdog` is a hosted, real-time Model Context Protocol (MCP) validator that invisible audits every CSS change an agent makes, forcing it to catch and correct its own "visual drift" against your strict design tokens before it hits your codebase.

---

## The Core Concept

<p align="center">
  <img src="https://github.com/swatchdog-dev/swatchdog-antigravity-bridge/raw/main/swatchdog-handshake-visual.png" width="80%" alt="Visual drift loop">
</p>

1. **Agent hallucinations a rogue layout** (e.g., raw hex `#FF00FF` instead of your semantic `color.primary`).
2. **Swatchdog MCP Validator intercepts the write.**
3. **Validator rejects the non-compliant code** with precise line references.
4. **Agent receives Swatchdog's corrections** and autonomously refactors the CSS using valid tokens.

---

## Quick Start (v0.1.0)

For Antigravity 2.0 Desktop or Cursor local workspaces:

1. **Install the Configuration:** Create a `.agents` folder at your project root and create `mcp_config.json`.
2. **Add Your Swatchdog Key:** Paste your connection snippet:

```json
{
  "mcpServers": {
    "swatchdog-validator": {
      "serverUrl": "[https://swatchdog-mcp.run.app/mcp/](https://swatchdog-mcp.run.app/mcp/)",
      "headers": {
        "Authorization": "Bearer YOUR_MINTED_KEY"
      }
    }
  }
}
