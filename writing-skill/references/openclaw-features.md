# OpenClaw-Specific Features

Features specific to OpenClaw skill system.

## Skill Locations & Priority

Skills load from three locations (highest to lowest priority):

1. `<workspace>/skills/` - Workspace skills (highest)
2. `~/.openclaw/skills/` - Managed/local skills
3. Built-in skills (bundled with OpenClaw)

**Priority rule**: Workspace skills override managed skills override built-in.

## Gating (Load-Time Filtering)

Skills can be conditionally loaded based on environment.

### Binaries Required

```yaml
metadata: { "openclaw": { "requires": { "bins": ["python3", "pdftk"] } } }
```

Skill only loads if all binaries exist in PATH.

### Environment Variables Required

```yaml
metadata: { "openclaw": { "requires": { "env": ["OPENAI_API_KEY"] } } }
```

Skill only loads if env var is set OR provided in config.

### Config Path Required

```yaml
metadata: { "openclaw": { "requires": { "config": ["browser.enabled"] } } }
```

Skill only loads if `browser.enabled` is truthy in openclaw.json.

### Any Binary (At Least One)

```yaml
metadata: { "openclaw": { "requires": { "anyBins": ["uv", "pip"] } } }
```

Skill loads if at least one binary exists.

### Platform Filter

```yaml
metadata: { "openclaw": { "os": ["darwin", "linux"] } }
```

Skill only loads on macOS and Linux (not Windows).

### Skip All Gating

```yaml
metadata: { "openclaw": { "always": true } }
```

Skill always loads regardless of other conditions.

## Configuration in openclaw.json

Users can configure skills in `~/.openclaw/openclaw.json`:

```json
{
  "skills": {
    "entries": {
      "my-skill": {
        "enabled": true,
        "apiKey": "API_KEY_HERE",
        "env": {
          "MY_API_KEY": "API_KEY_HERE"
        },
        "config": {
          "endpoint": "https://example.com"
        }
      }
    }
  }
}
```

### Configuration Fields

| Field | Description |
|-------|-------------|
| `enabled` | `true`/`false` - Toggle skill on/off |
| `apiKey` | Convenience field for primary API key |
| `env` | Environment variables to inject |
| `config` | Custom skill-specific settings |

### Primary Env Var

Link `apiKey` to a specific env var:

```yaml
metadata: { "openclaw": { "primaryEnv": "GEMINI_API_KEY" } }
```

Then in config:
```json
"gemini-skill": {
  "apiKey": "your-gemini-key"
}
```

This sets `GEMINI_API_KEY` when the skill runs.

## User-Invocable Skills

Control whether skill appears as slash command:

```yaml
metadata: { "openclaw": { "user-invocable": true } }
```

Default is `true`. Set to `false` to hide from slash commands.

## Disable Model Invocation

Prevent skill from being loaded into model context:

```yaml
metadata: { "openclaw": { "disable-model-invocation": true } }
```

Skill still available via slash command, but not auto-loaded.

## Direct Tool Dispatch

Route slash command directly to a tool (bypass model):

```yaml
metadata: { "openclaw": { "command-dispatch": "tool", "command-tool": "my_tool" } }
```

Tool receives:
```json
{ "command": "<raw args>", "commandName": "<slash command>", "skillName": "<skill name>" }
```

## Installers (macOS Skills UI)

Define installation methods for macOS:

```yaml
metadata: { "openclaw": { "install": [
  { "id": "brew", "kind": "brew", "formula": "my-cli", "bins": ["my-cli"] },
  { "id": "npm", "kind": "node", "package": "my-cli", "bins": ["my-cli"] }
] } }
```

### Installer Types

| Kind | Fields |
|------|--------|
| `brew` | `formula`, `bins` |
| `node` | `package`, `bins` |
| `go` | `package`, `bins` |
| `download` | `url`, `archive`, `extract`, `targetDir` |

## Token Cost Estimation

When skills are loaded, they add to context:

- **Base overhead**: ~195 characters (when ≥1 skill)
- **Per skill**: ~97 characters + name + description + location

Rough estimate: ~24 tokens per skill + content length.

## Skill Monitor (Hot Reload)

OpenClaw watches skill folders for changes:

```json
{
  "skills": {
    "load": {
      "watch": true,
      "watchDebounceMs": 250
    }
  }
}
```

Changes to `SKILL.md` automatically refresh the skill list.

## ClawHub Integration

Share and discover skills at https://clawhub.com

```bash
# Install a skill
clawhub install <skill-slug>

# Update all skills
clawhub update --all
```

Default install location: `./skills` in current directory or workspace.
