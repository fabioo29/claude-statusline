# Claude Custom Status Line

A customized Claude Code status line with enhanced features.

## Features

### 1. Shorter Progress Bar
- Reduced context progress bar from 10 to 5 characters for a more compact display

### 2. Dynamic 7-Day Rate Limit Colors
Instead of static color thresholds, the 7-day rate limit now uses **daily budget tracking**:

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
- **Green**: Usage ≤ `(day - 0.5) × 14.28%` (under half-day budget for current day)
- **Yellow**: Usage ≤ `day × 14.28%` (within daily budget)
- **Red**: Usage > `day × 14.28%` (over budget for current day)

This helps you pace your usage throughout the week instead of using static 50%/80% thresholds.

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

This is a customized fork of the [claude-dashboard](https://github.com/anthropics/claude-code) plugin.

## License

MIT
