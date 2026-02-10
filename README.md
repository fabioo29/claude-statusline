# Claude Custom Status Line

A lightweight, Claude-only status line for Claude Code. Stripped of Codex, Gemini, and z.ai integrations.

## Features

### 1. Compact Progress Bar
- 5-character progress bar (reduced from 10)

### 2. Dynamic 7-Day Rate Limit Colors
The 7-day rate limit uses **daily budget tracking** instead of static thresholds:

| Day | Green (on track) | Yellow (warning) | Red (over budget) |
|-----|------------------|------------------|-------------------|
| 1   | 0-6%             | 7-13%            | 14%+              |
| 2   | 0-20%            | 21-27%           | 28%+              |
| 3   | 0-35%            | 36-42%           | 43%+              |
| 4   | 0-49%            | 50-56%           | 57%+              |
| 5   | 0-64%            | 65-71%           | 72%+              |
| 6   | 0-78%            | 79-85%           | 86%+              |
| 7   | 0-92%            | 93-99%           | 100%              |

**Logic (with floor for stricter thresholds):**
- **Green**: Usage < `floor((day - 0.5) × 14.28%)` (under budget)
- **Yellow**: Usage < `floor(day × 14.28%)` (at budget)
- **Red**: Usage ≥ `floor(day × 14.28%)` (over budget)

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
