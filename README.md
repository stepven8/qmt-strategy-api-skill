# QMT Strategy API Skill for Codex

This repository contains a Codex skill for QMT/迅投极速策略交易系统 quantitative strategy development.

The skill guides Codex to use the bundled QMT API reference when writing, reviewing, debugging, or porting Python strategies. It covers `handlebar` K-line strategies, quote subscription callbacks, scheduled tasks, order placement, account/position/order/deal queries, market data fetching, financial data, and backtest-vs-live API differences.

## Install

Prerequisites:

- Codex is already installed.
- Python 3 is available from your terminal.

### macOS / Linux

Run this command in Terminal:

```bash
python3 ~/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py \
  --repo stepven8/qmt-strategy-api-skill \
  --path skills/qmt-strategy-api
```

### Windows

Run this command in PowerShell:

```powershell
python "$env:USERPROFILE\.codex\skills\.system\skill-installer\scripts\install-skill-from-github.py" `
  --repo stepven8/qmt-strategy-api-skill `
  --path skills/qmt-strategy-api
```

If `python` is not recognized on Windows, try `py` instead:

```powershell
py "$env:USERPROFILE\.codex\skills\.system\skill-installer\scripts\install-skill-from-github.py" `
  --repo stepven8/qmt-strategy-api-skill `
  --path skills/qmt-strategy-api
```

Restart Codex after installation so the new skill is loaded.

The installed skill should appear at:

- macOS / Linux: `~/.codex/skills/qmt-strategy-api`
- Windows: `%USERPROFILE%\.codex\skills\qmt-strategy-api`

## Use

After restarting Codex, invoke the skill by name:

```text
$qmt-strategy-api
```

Example prompt:

```text
Use $qmt-strategy-api to write a QMT handlebar strategy that checks signals only on the latest bar.
```

## Contents

- `skills/qmt-strategy-api/SKILL.md`: Skill instructions and usage workflow.
- `skills/qmt-strategy-api/references/qmt_api_full.md`: Local QMT API reference used by the skill.
- `skills/qmt-strategy-api/agents/openai.yaml`: Codex display metadata.

