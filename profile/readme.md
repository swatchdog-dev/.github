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

## Using the free sandbox

Your sandbox key unlocks the **Studio** family — five colorways. Use one as your `paletteId`:

| paletteId | Look |
|---|---|
| `studio-ink-room` | deep indigo-blue on cream |
| `studio-blue-hour` | moody dark blue |
| `studio-acid-print` | electric green on pale chartreuse |
| `studio-clay-gallery` | warm terracotta |
| `studio-rose-archive` | soft rose / plum |

**The loop (works in any MCP-aware agent):**
1. **See what's available** — read `swatchdog://catalog` (a free key lists the Studio packs).
2. **Load the standard** — read a pack's tokens, e.g. `swatchdog://pack/studio-blue-hour/tokens`.
3. **Build** your UI using those tokens.
4. **Check** — call `check_drift("studio-blue-hour", content="<your CSS/markup>")`.
   You get structured findings on declared color/radius/spacing/type tokens, each with a
   suggested token fix. On-demand — the loop is yours.

**Want a different look?** The full catalog (Workbench, Showcase, Terminal) is at
**swatchdog.dev** — $19 per family or $49 all-in. Call `check_drift` against a family you
don't have and swatchdog points you to the upgrade.

## Connect

Works with any MCP-aware agent (Claude Code, Cursor, Antigravity, and other MCP
clients). Add an HTTP MCP server with the shared sandbox key:

```jsonc
{
  "mcpServers": {
    "swatchdog-sandbox": {
      "type": "http",
      "url": "https://swatchdog-mcp-970396648818.us-central1.run.app/mcp",
      "headers": { "Authorization": "Bearer swt_sandbox_studio" }
    }
  }
}
