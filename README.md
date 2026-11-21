# my-agent-skills

Personal Claude Code skills. Each folder is one skill.

## Install

Use the [`skills`](https://github.com/vercel-labs/skills) CLI (needs Node.js; no global install required):

```bash
# Install to your user directory (available in every project)
npx skills add bakamitai456/my-agent-skills -g -a claude-code

# Install a single skill only
npx skills add bakamitai456/my-agent-skills --skill js-env-setup -g -a claude-code

# Install into the current project only (omit -g)
npx skills add bakamitai456/my-agent-skills -a claude-code
```

Add `-y` to skip confirmation prompts. Restart Claude Code sessions to pick up new skills.

Manage installed skills:

```bash
npx skills list -g
npx skills update
npx skills remove js-env-setup -g
```

### Manual install (alternative)

```bash
git clone https://github.com/bakamitai456/my-agent-skills.git ~/my-agent-skills
ln -s ~/my-agent-skills/js-env-setup ~/.claude/skills/js-env-setup
```

Update with `git pull`.

## Skills

- `js-env-setup`: selects the Node version with nvm and the package manager (pnpm by default).
