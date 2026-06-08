# Swatchdog Antigravity Bridge (Beta)

<!-- Add image_3.png here as a clean header -->
![Swatchdog Banner](swatchdog-github-banner.svg)

# swatchdog — free sandbox

**Give your AI agent a design standard to build from — then ask it to check its
own work against that standard, on demand, over MCP.**

swatchdog is a hosted, license-gated MCP server. Point your agent at a design
pack and call `check_drift`: swatchdog runs deterministic code against the
**declared** color, radius, spacing, and type tokens in the CSS/markup your agent
generated, and returns structured findings with suggested token fixes. The check
is yours to invoke — swatchdog doesn't intercept writes or enforce anything on its
own; the build-and-check loop runs in your agent.

> Checks declared styles and tokens, not pixels — no browser, no screenshots, no
> rendered inspection.

## Free sandbox: the Studio family

One full design family — **Studio** (5 colorways) — free, over MCP:

- **Guidance** — read the pack's tokens, rules, and CSS so your agent builds from
  a real standard.
- **`check_drift`** — run the live, deterministic check against what it built.

No download, no signup. The full catalog (Workbench, Showcase, Terminal) is at
**swatchdog.dev**.

## Connect

Works with any MCP-aware agent (Claude Code, Cursor, Antigravity, and other MCP
clients). Add an HTTP MCP server with the shared sandbox key:

```jsonc
{
  "mcpServers": {
    "swatchdog-sandbox": {
      "type": "http",
      "url": "https://<CLOUD-RUN-HOST>/mcp",
      "headers": { "Authorization": "Bearer swt_sandbox_studio" }
    }
  }
}
