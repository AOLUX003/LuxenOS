# Security Setup Guide - LuxenOS

This repository has comprehensive secret scanning protection at multiple layers.

## Quick Start

### 1. Install Pre-Commit Hooks (Local Protection)

```bash
# Install pre-commit tool
pip install pre-commit

# Install the hooks in this repository
cd /path/to/LuxenOS
pre-commit install

# Test it works
pre-commit run --all-files
```

### 2. Verify GitHub Actions (Remote Protection)

GitHub Actions automatically scan on:
- Every push to main/master/develop
- Every pull request
- Daily at 2 AM UTC

View workflow runs: https://github.com/AOLUX003/LuxenOS/actions

## Protection Layers

### Layer 1: Local Pre-Commit Hooks
**Blocks commits BEFORE they leave your machine**

- TruffleHog secret detection
- Gitleaks scanning
- .env file blocking
- MCP configuration file blocking
- Backup file detection
- API key pattern matching (Anthropic, OpenAI, GitHub, AWS)
- .gitignore validation

### Layer 2: GitHub Actions Workflow
**Scans code in GitHub on every push/PR**

- TruffleHog (verified secrets only)
- Gitleaks comprehensive scan
- File checks (.env, MCP configs, backups)
- Pattern matching (API keys, tokens)
- .gitignore validation

### Layer 3: Daily Automated Scans
**Catches anything that slipped through**

- Runs automatically at 2 AM UTC
- Full repository history scan
- Email notifications on findings

## What Gets Blocked

### Files
- `.env`, `.env.*` (environment files)
- `mcp.json`, `claude_desktop_config.json` (MCP configs)
- `*.bak`, `*.old`, `*.tmp` (backup files)
- `credentials.json`, `secrets.json`
- `*.key`, `*.pem` (key files)

### Patterns
- GitHub tokens: `ghp_`, `gho_`, `ghs_`, `ghu_`, `ghr_`
- Anthropic API keys: `sk-ant-api##-...`
- OpenAI API keys: `sk-...` (48 chars)
- AWS Access Keys: `AKIA...`
- Generic API keys and passwords
- Bearer tokens

## Best Practices

1. **Never commit secrets** - Use environment variables
2. **Use .env.example** - Template without real values
3. **Rotate exposed secrets immediately**
4. **Review .gitignore** - Ensure it includes all sensitive patterns
5. **Run pre-commit before pushing** - `pre-commit run --all-files`

## MCP Configuration Security

MCP tokens should NEVER be committed to git. Instead:

### Development
```bash
# Store in user config directory (already ignored by git)
# Windows: %APPDATA%/Claude/
# macOS: ~/Library/Application Support/Claude/
# Linux: ~/.config/claude/
```

---

🤖 Generated with [Claude Code](https://claude.com/claude-code)
