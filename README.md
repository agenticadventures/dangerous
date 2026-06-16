# Isolated Claude Code agent (never-prompted)

Sandbox repo for running fully autonomous, **never-prompted** Claude Code agents in a GitHub
Codespace. The agent runs on a throwaway cloud VM (not your computer), can reach only this repo
plus the internet, and draws on a budget-capped Anthropic key. Because permission prompts are
turned fully off, the *environment* is the safety boundary — that's what the isolation is for.

## Launch the agent

1. Click **Code → Codespaces → Create codespace**, and open it **in the browser**.
2. In the terminal, confirm the key is present:
   ```bash
   echo ${ANTHROPIC_API_KEY:+set}   # should print: set
   ```
3. Start it (the alias already adds `--dangerously-skip-permissions`):
   ```bash
   claude
   ```
   Accept the one-time warning the first time (it won't reappear on rebuilds thanks to the
   persisted config volume). Then type prompts at the `>` REPL prompt — no further approvals.

For unattended, one-shot runs:
```bash
claude -p "your task here"
```

## What's in here

- `.devcontainer/devcontainer.json` — installs Claude Code, aliases `claude` to run unprompted,
  and persists `~/.claude` across rebuilds.
- `.claude/settings.json` — project setting `defaultMode: bypassPermissions` (documented intent;
  the alias is the reliable enforcer).
- `.github/workflows/pages.yml` — publishes the site on push to `main`.
- `index.html` — placeholder so Pages works on the first deploy; the agent can replace it.

## One-time setup (in the GitHub UI)

- **Settings → Secrets and variables → Codespaces** → add `ANTHROPIC_API_KEY`
  (a workspace-scoped, spend-capped key from the Anthropic Console).
- **Settings → Pages → Source** → set to **GitHub Actions**.

Live site: `https://<your-username>.github.io/<repo>/`

See `SETUP-GUIDE.md` for full account creation, budget caps, and the isolation checklist.

## Note

`bypassPermissions` provides no protection against prompt injection or unintended actions; it only
removes prompts. The throwaway account, ephemeral VM, repo-only token, and capped key are what
keep a runaway agent contained.
