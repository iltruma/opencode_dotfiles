# opencode dotfiles

My personal [opencode](https://opencode.ai) configuration, synced across machines.

## What's inside

| File | Description |
|---|---|
| `opencode.json` | Base config — agents, MCP servers, plugins, permissions |
| `rules.md` | Global instructions injected into every session |
| `tui.json` | TUI preferences |
| `agents/analyst.md` | Default agent — SRE analysis, infra diagnostics, log analysis, troubleshooting (read-only) |
| `agents/architect.md` | Architecture research and technical planning (read-only, subagent) |
| `agents/coder.md` | Senior software engineer — writes and edits code, runs builds and tests. Uses `opencode-go/minimax-m3` by default (override in `opencode.local.json` if the provider is unavailable) |
| `agents/documenter.md` | Maintains existing documentation — updates README, CHANGELOG, docstrings (existing files only, no new docs) |
| `agents/reviewer.md` | Senior code review — bugs, edge cases, maintainability, security, performance (read-only, subagent) |

> Built-in agents `build`, `plan`, `general`, and `explore` are disabled in `opencode.json` — only the custom agents listed above are available.

## MCP servers

| Server | Type | Purpose |
|---|---|---|
| [context7](https://context7.com) | remote | Up-to-date library documentation |
| [exa](https://exa.ai) | remote | Web search |
| [grep.app](https://grep.app) | remote | Code search across public GitHub repos |
| [sequential-thinking](https://github.com/modelcontextprotocol/servers/tree/main/src/sequentialthinking) | local | Structured reasoning |
| [codemem](https://github.com/kunickiaj/codemem) | local | Persistent memory across sessions — **managed via plugin** (`@codemem/opencode-plugin`), non come MCP server separato |
| [playwright](https://github.com/microsoft/playwright-mcp) | local | Browser automation (disabled by default) |

## Plugins

Plugins are npm packages listed in `opencode.json` under the top-level `plugin` key.
opencode uses its **embedded bun** to resolve and load them at startup; this repo ships two.

| Plugin | Purpose | Notes |
|---|---|---|
| [`@dietrichgebert/ponytail`](https://github.com/DietrichGebert/ponytail) | "Lazy senior dev" ruleset injected into every turn, plus `/ponytail*` commands | Always-on by default. Requires `node` on PATH for lifecycle hooks. Default mode `full` — override per-machine via `PONYTAIL_DEFAULT_MODE` env (`lite`/`full`/`ultra`/`off`) or `defaultMode` in `~/.config/ponytail/config.json`. |
| [`@codemem/opencode-plugin`](https://github.com/kunickiaj/codemem) | Persistent memory across sessions — context summaries and observations surfaced at session start | Gestito come plugin (non MCP server separato). Richiede il binario `codemem` globale — vedi note sotto. |

### codemem — note operative

Il plugin richiede il binario `codemem` sul PATH. Installarlo globalmente con **bun**:

```bash
bun install -g codemem@0.22.4
```

Senza il binario, all'avvio compare il warning `/bin/sh: codemem: not found`.

**Viewer pid stale**: se il viewer crasha e lascia un file stale, il plugin non si riavvia finché non si cancella manualmente:

```bash
rm ~/.codemem/viewer.pid
```

## Setup

### Prerequisites

- [opencode](https://opencode.ai/docs) installed
- `bun` installed (`curl -fsSL https://bun.sh/install | bash`)
- `uv` installed (`curl -LsSf https://astral.sh/uv/install.sh | sh`)
- MCP local servers e binari installati:

```bash
# Node-based (via bun)
bun install -g mcp-server-sequential-thinking
bun install -g @playwright/mcp  # installs the 'playwright-mcp' binary
bun install -g codemem@0.22.4   # required by @codemem/opencode-plugin
```

### New machine

```bash
# 1. Clone the repo
git clone git@github.com:iltruma/opencode_dotfiles.git ~/dotfiles/opencode_dotfiles

# 2. Back up existing config (if any)
mv ~/.config/opencode ~/.config/opencode.bak 2>/dev/null || true

# 3. Symlink the config folder
ln -s ~/dotfiles/opencode_dotfiles ~/.config/opencode

 # 4. Install plugins (opencode uses embedded bun automatically at startup)
 #    No manual install needed — bun.lockb is committed and pins versions
 cd ~/.config/opencode && bun install
```

### Provider setup

This repo does **not** include any provider configuration — API keys and provider
details are machine-specific and never committed.

#### Option A — opencode Go (personal machine)

Run the `/connect` command inside the opencode TUI, select **OpenCode Go**,
and paste your API key. Done.

#### Option B — Custom provider (work machine)

Create a local override file (never committed):

```bash
cat > ~/.config/opencode/opencode.local.json << 'EOF'
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {
    "your-provider": {
      "npm": "@ai-sdk/openai-compatible",
      "name": "Your Provider",
      "options": {
        "baseURL": "https://your-provider-url/v1",
        "apiKey": "your-api-key"
      },
      "models": {
        "model-id": {
          "name": "Model Display Name",
          "limit": { "context": 200000, "output": 65536 }
        }
      }
    }
  }
}
EOF
```

Then export the path in your `~/.zshrc`:

```bash
export OPENCODE_CONFIG="$HOME/.config/opencode/opencode.local.json"
```

opencode will deep-merge the local file on top of the base config at startup.

## Daily workflow

```bash
# Edit any config file — changes are already in ~/dotfiles/opencode_dotfiles via symlink

# Sync changes
cd ~/dotfiles/opencode_dotfiles
git add .
git commit -m "..."
git push

# Pull updates on another machine
cd ~/dotfiles/opencode_dotfiles && git pull
```

## What is NOT committed

| File/Folder | Reason |
|---|---|
| `opencode.local.json` | Machine-specific provider config and API keys |
| `node_modules/` | Generated by bun |
| `package.json` | Auto-generated by opencode for plugins |
| `bun.lockb` | Generated by bun — **non committare**, machine-specific |
| `package-lock.json` | Legacy npm lockfile (sostituito da `bun.lockb`) |
| `opencode-quota/` | Usage data generated by opencode at runtime |
