# Claude Custom Status Line

A lightweight, Claude-only status line for Claude Code. Stripped of Codex, Gemini, and z.ai integrations.

## Features

### 1. Compact Progress Bar
- 5-character progress bar (reduced from 10)

### 2. Dynamic 7-Day Rate Limit Colors
The 7-day rate limit uses **daily budget tracking** instead of static thresholds:

| Day | Green (on track) | Yellow (half-day over) | Red (over budget) |
|-----|------------------|------------------------|-------------------|
| 1   | ≤7.14%           | ≤14.28%                | >14.28%           |
| 2   | ≤21.42%          | ≤28.57%                | >28.57%           |
| 3   | ≤35.71%          | ≤42.86%                | >42.86%           |
| 4   | ≤50.00%          | ≤57.14%                | >57.14%           |
| 5   | ≤64.29%          | ≤71.43%                | >71.43%           |
| 6   | ≤78.57%          | ≤85.71%                | >85.71%           |
| 7   | ≤92.86%          | ≤100%                  | >100%             |

**Logic:**
- **Green**: Usage ≤ `(day - 0.5) × 14.28%` (under budget)
- **Yellow**: Usage ≤ `day × 14.28%` (at budget)
- **Red**: Usage > `day × 14.28%` (over budget)

### 3. Claude-Only (Lightweight)
Removed all non-Claude integrations:
- ~~Codex CLI~~
- ~~Gemini CLI~~
- ~~z.ai / ZHIPU~~

**Result:** 1507 lines (down from 2481)

## Installation

### 1. Copy the status line script
```bash
mkdir -p ~/.claude/statusline
cp dist/index.js ~/.claude/statusline/
```

### 2. Update your settings.json
Edit `~/.claude/settings.json` and set the statusLine:
```json
{
  "statusLine": {
    "type": "command",
    "command": "node ~/.claude/statusline/index.js"
  }
}
```

### 3. (Optional) Copy config example
```bash
cp claude-dashboard.local.json.example ~/.claude/claude-dashboard.local.json
```

## Configuration

Edit `~/.claude/claude-dashboard.local.json` to customize widgets:

```json
{
  "language": "auto",
  "plan": "max",
  "displayMode": "custom",
  "lines": [
    ["model", "context", "rateLimit5h", "rateLimit7d"]
  ],
  "cache": {
    "ttlSeconds": 60
  }
}
```

### Available Widgets
- `model` - Model name with emoji
- `context` - Progress bar, percentage, tokens
- `cost` - Session cost in USD
- `rateLimit5h` - 5-hour rate limit
- `rateLimit7d` - 7-day rate limit (with dynamic colors)
- `rateLimit7dSonnet` - 7-day Sonnet limit (with dynamic colors)
- `projectInfo` - Directory + git branch
- `sessionDuration` - Session duration
- `todoProgress` - Todo completion rate
- `burnRate` - Token consumption per minute
- `cacheHit` - Cache hit percentage

## Based On

This is a customized version of the claude-dashboard plugin.

## License

MIT
