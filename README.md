# 🧠 Analyze Claude Tokens

[![Python 3.10+](https://img.shields.io/badge/python-3.10%2B-blue.svg)](https://www.python.org/downloads/)
[![Tests](https://img.shields.io/badge/tests-passing-brightgreen.svg)](#-tests)
[![License: MIT](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)
[![Ask DeepWiki](https://deepwiki.com/badge.svg)](https://deepwiki.com/informigados/analyze-claude-tokens)

Analyze local Claude session transcripts and generate usage reports.

> ℹ️ **Claude** is an Anthropic product.  
> This project is an independent local analyzer and is not an official Anthropic repository.

## 🚀 What It Does

The analyzer scans local `*.jsonl` session files, then generates:

- token usage totals (input, cached input, output, reasoning)
- usage by project
- costly sessions and subagents
- prompt extraction by project
- markdown + JSON report output

It supports both:

- legacy JSONL event format (`session_meta`, `event_msg`, `token_count`)
- Claude-style transcript JSONL records (generic parser with usage/prompt extraction)

## 🗂️ Default Search Paths

By default, the tool scans these common directories:

- `~/.claude/projects`
- `~/.claude/sessions`
- `~/.config/claude/projects`
- `~/.config/claude/sessions`
- Windows: `%APPDATA%\Claude\local-agent-mode-sessions`
- Windows: `%LOCALAPPDATA%\AnthropicClaude\local-agent-mode-sessions`

You can override/add paths with:

- `--claude-home PATH` (changes the base `~/.claude` equivalent)
- `--session-dir PATH` (repeatable, adds extra transcript folders)
- `CLAUDE_SESSION_DIRS` env var (multiple folders separated by OS path separator)

Examples:

```powershell
py analyze-claude-tokens.py --session-dir "D:\ClaudeLogs" --session-dir "E:\Backups\Claude"
```

```powershell
$env:CLAUDE_SESSION_DIRS="D:\ClaudeLogs;E:\Backups\Claude"
py analyze-claude-tokens.py
```

## ⚙️ Requirements

- Python 3.10+
- No external dependencies

## ▶️ Run

```bash
python analyze-claude-tokens.py
```

Windows PowerShell:

```powershell
py analyze-claude-tokens.py
```

## 📂 Output

Default output directory:

```text
./reports/<lang>-YYYY-MM-DD_HHMMSS/
```

Generated files:

- `token_report.md`
- `token_report.json` (unless disabled)
- `prompts/*.md`

## 🧩 CLI Options

```bash
python analyze-claude-tokens.py \
  --since-days 7 \
  --lang pt-br \
  --output-dir ./reports \
  --redact-prompts \
  --json
```

Available flags:

- `--since-days N`
- `--since-date YYYY-MM-DD`
- `--claude-home PATH`
- `--output-dir PATH`
- `--session-dir PATH` (repeatable)
- `--lang en|pt-br|pt-pt|es`
- `--redact-prompts` / `--no-redact-prompts`
- `--json` / `--no-json`

## 🔧 Environment Variables

- `CLAUDE_HOME` (primary)
- `OUTPUT_DIR`
- `CLAUDE_SESSION_DIRS` (extra directories separated by `;` on Windows, `:` on Linux/macOS)
- `SINCE_DAYS`
- `SINCE_DATE`
- `REDACT_PROMPTS`
- `WRITE_JSON`
- `REPORT_LANG`

Example:

```powershell
$env:CLAUDE_HOME="C:\Users\you\.claude"
py analyze-claude-tokens.py --lang pt-br
```

## ✅ Tests

```bash
python -m unittest discover -s tests -v
```

PowerShell:

```powershell
py -m unittest discover -s tests -v
```

## ⚠️ Notes

- Only sessions with `total_tokens > 0` are included.
- Some Claude Desktop data may be stored in cache/indexed DB formats not directly parseable as JSONL.
- If no data is found, the script now prints all scanned directories; use `--session-dir`/`CLAUDE_SESSION_DIRS` to point to your real transcript locations.
- Report language supports `en`, `pt-br`, `pt-pt`, and `es`; unsupported locale values gracefully fall back to English.

## 📚 Data Source Reference

Claude Code docs show session events exposing `transcript_path` under `~/.claude/projects/.../*.jsonl`, which matches this analyzer’s primary source strategy:

- https://code.claude.com/docs/en/hooks

## 📝 Changelog

### 2026-04-10 (1.0.0)

- Initial release.

## 👥 Authors

- INformigados: https://github.com/informigados/
- Alex Brito: https://github.com/alexbritodev

## 📜 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
