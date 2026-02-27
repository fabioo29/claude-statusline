# claude-statusline

A patched [claude-dashboard](https://github.com/mbenford/claude-dashboard) (v1.10.0) status line for Claude Code.

## Features

### Account identification

Displays the logged-in account name with an icon based on the email domain:

Work account (`@company.com`):
```
💼 Jane Doe │ 📁 my-project (main) │ 🤖 Opus(H)
```

Personal account (`@gmail.com`):
```
👤 Jane Doe │ 📁 side-project (dev) │ 🤖 Opus(H)
```

### Dynamic 7-day rate limit colors

The `7d: X%/Y%` display shows your current usage against a daily budget threshold. Colors shift based on what day of the 7-day window you're on:

Day 1 (~14% budget):
```
7d: 5%/14%    <- green, well under budget
7d: 12%/14%   <- yellow, approaching budget
7d: 18%/28%   <- red, over budget (threshold auto-advances)
```

Day 4 (~57% budget):
```
7d: 30%/57%   <- green
7d: 50%/57%   <- yellow
7d: 60%/71%   <- red
```

| Color | Meaning |
|---|---|
| Green | Under half of today's daily budget |
| Yellow | Approaching today's daily budget |
| Red | Over today's daily budget |

### 5-hour rate limit colors

Fixed thresholds:

```
5h: 30%    <- green (under 50%)
5h: 65%    <- yellow (50-80%)
5h: 90%    <- red (over 80%)
```

### Full status line example

```
💼 Jane Doe │ 📁 my-project (main) │ 🤖 Opus(H)
█████░░░░░ │ 48% │ 96K/200K │ 5h: 32% (3h12m) │ 7d: 8%/14% (5d18h)
```

## Installation

### 1. Copy the files

```bash
mkdir -p ~/.claude/statusline
cp index.js ~/.claude/statusline/
cp claude-dashboard.local.json ~/.claude/
```

### 2. Configure Claude Code

Add the `statusLine` key to `~/.claude/settings.json`:

```json
{
  "statusLine": {
    "type": "command",
    "command": "node $HOME/.claude/statusline/index.js"
  }
}
```

### 3. Restart Claude Code

Close and reopen your session. The status line should appear at the bottom of the terminal.

## Configuration

Edit `~/.claude/claude-dashboard.local.json` to customize the layout:

```json
{
  "language": "auto",
  "plan": "max",
  "displayMode": "custom",
  "lines": [
    ["accountName", "projectInfo", "model"],
    ["context", "rateLimit5h", "rateLimit7d"]
  ],
  "cache": {
    "ttlSeconds": 60
  }
}
```

## License

MIT
