# Coreflux AI Starter

Pre-built AI assistant configuration for Coreflux LoT (Language of Things) development. Copy a template into your project, connect the MCP, and start building IIoT solutions with AI — correctly, from the first prompt.

## Quick Start

1. **Copy `AGENTS.md`** (or `CLAUDE.md`) into your project root
2. **Copy your editor's MCP config** from the `cursor/`, `vscode/`, or `claude-desktop/` folder
3. **Start prompting** — your AI assistant now knows LoT syntax, naming conventions, and best practices

That's it. Your AI assistant will follow the conventions in the file and consult the Coreflux documentation through the MCP for accurate syntax.

## What's Inside

```
├── cursor/                          # Cursor IDE configuration
│   ├── AGENTS.md                    # Universal template (Cursor, Copilot, Codex, Jules, Aider)
│   ├── mcp.json                     # MCP server connection (copy to .cursor/)
│   └── rules/
│       └── lot-conventions.mdc      # Cursor-native rules with glob support
│
├── vscode/                          # VS Code + GitHub Copilot configuration
│   ├── AGENTS.md                    # Universal template (Cursor, Copilot, Codex, Jules, Aider)
│   └── mcp.json                     # MCP server connection (copy to .vscode/)
│
├── claude-desktop/                  # Claude Desktop configuration
│   ├── CLAUDE.md                    # Claude Code variant (same content, expected filename)
│   └── claude_desktop_config.json   # MCP server connection
│
└── templates/                       # AGENTS.md variants for different needs
    ├── agents-minimal.md            # Bare-minimum starter (~30 lines)
    ├── agents-full.md               # Complete template with all conventions
    ├── agents-industrial.md         # Extended for PLC/OT-heavy projects
    └── examples/                        # Filled-in examples showing real usage
        ├── temperature-monitoring/      # Basic sensor monitoring project
        └── plc-to-database/             # Industrial data pipeline project  

```

## MCP Setup by Editor

The Coreflux MCP (Model Context Protocol) gives your AI assistant real-time access to the official LoT documentation. Without it, the AI may invent plausible-looking but incorrect syntax.

### Cursor

Copy `cursor/mcp.json` to `.cursor/mcp.json` in your project (or `~/.cursor/mcp.json` for global). Restart Cursor and verify under **Settings → MCP**.

### VS Code (GitHub Copilot)

Copy `vscode/mcp.json` to `.vscode/mcp.json` in your workspace. Reload VS Code. The tools become available in Copilot's **Agent** mode.

### Claude Desktop

Merge the contents of `claude-desktop/claude_desktop_config.json` into your Claude Desktop config file:

| OS | Config Location |
|----|----------------|
| Windows | `%APPDATA%\Claude\claude_desktop_config.json` |
| macOS | `~/Library/Application Support/Claude/claude_desktop_config.json` |
| Linux | `~/.config/Claude/claude_desktop_config.json` |

Restart Claude Desktop. The Coreflux tools appear in the tool picker (hammer icon).

### Claude Code

Claude Code reads `CLAUDE.md` from the project root automatically. Copy `CLAUDE.md` into your project — no additional MCP configuration needed beyond the file itself.

## Choosing a Template

| Template | Best For | Size |
|----------|----------|------|
| **`agents-minimal.md`** | Quick experiments, learning LoT, small projects | ~30 lines |
| **`agents-full.md`** | Production projects, teams, consistent AI output | ~70 lines |
| **`agents-industrial.md`** | PLC integration, OT/IT pipelines, factory automation | ~100 lines |

Start with **minimal** if you're exploring. Move to **full** when you start a real project. Use **industrial** when your system includes PLCs, industrial protocols, or OT network segmentation.

## Customization

Every template includes placeholder sections marked with `[brackets]`. Fill these in:

- `[describe your system]` — A one-liner about your project (e.g., "Factory floor temperature monitoring with S7 PLCs")
- `[your topic namespaces]` — Add project-specific topic prefixes if you use custom ones
- `[your thresholds]` — Add domain-specific rules (e.g., "temperature alerts above 75°C")

The templates are starting points. Add rules when you notice the AI making a mistake — that's the most effective way to improve results over time.

## Examples

The `examples/` folder contains complete project setups with filled-in AGENTS.md files. These show what a "real" AGENTS.md looks like — not the generic template, but one tailored to a specific use case.

- **`temperature-monitoring/`** — Sensor data collection, unit conversion, threshold alerting
- **`plc-to-database/`** — Siemens S7 PLC reads, MQTT bridging, PostgreSQL storage

Each example includes a README explaining the project context and how the AGENTS.md was customized.

## Documentation

- [Using AI with Coreflux](https://docs.coreflux.org/quick-start/using-ai) — Quick-start guide
- [Developing with LoT Using AI](https://docs.coreflux.org/ai/developing-with-lot) — Full AI-assisted workflow
- [Best Practices & AGENTS.md](https://docs.coreflux.org/ai/best-practices) — Conventions reference
- [MCP Configuration Guide](https://docs.coreflux.org/ai/mcp) — Detailed MCP setup

## License

This project is licensed under the [Apache License 2.0](LICENSE).
