# Installing UI/UX Pro Max in AutoClaw

This guide covers three install methods for AutoClaw users. Pick whichever fits your workflow.

## Prerequisites

- AutoClaw installed (download from <https://autoclaw.z.ai/?channel=zai>)
- Python 3.x on your system (only standard library used by the search script)
- (Optional) Node.js + npm if you want to use the CLI installer

---

## Method 1: CLI Installer (Recommended)

Once the upstream PR is merged, the official CLI will support AutoClaw directly.

```bash
# Install the CLI globally
npm install -g ui-ux-pro-max-cli

# Install the skill into AutoClaw's skill directory
uipro init --ai autoclaw

# Or install globally for all your projects
uipro init --ai autoclaw --global
```

After install:

1. Open AutoClaw
2. Go to **Settings → Skills**
3. Click **Refresh**
4. Confirm `ui-ux-pro-max` appears with the toggle enabled
5. Start a chat with a design prompt (e.g., "Build a dashboard for healthcare analytics") - the skill will trigger automatically

---

## Method 2: Manual Install (Before PR Merge)

If the PR hasn't been merged yet, you can install from your own fork:

```bash
# 1. Fork the repo on GitHub, then clone your fork
git clone https://github.com/<your-username>/ui-ux-pro-max-skill.git

# 2. Create AutoClaw's skill directory (if it doesn't exist)
mkdir -p ~/.autoclaw/skills/

# 3. Copy the skill into place
cp -r ui-ux-pro-max-skill ~/.autoclaw/skills/ui-ux-pro-max

# 4. Verify the SKILL.md exists
ls ~/.autoclaw/skills/ui-ux-pro-max/SKILL.md
```

Then refresh AutoClaw's skill list as above.

---

## Method 3: Single File (Minimal)

If you only want the skill logic without the full data catalogs, you can drop just the `SKILL.md` file from this PR into AutoClaw's skill directory:

```bash
mkdir -p ~/.autoclaw/skills/ui-ux-pro-max/
# Copy the SKILL.md from this PR into that directory
cp SKILL.md ~/.autoclaw/skills/ui-ux-pro-max/SKILL.md
```

Note: With this minimal install, the search script (`scripts/search.py`) won't be available, so the skill will fall back to using its own embedded knowledge instead of querying the catalog files. You'll still get design system generation, but with reduced catalog depth.

---

## Verifying the Install

Open a new AutoClaw chat and try one of these prompts:

- "Build a landing page for my SaaS product"
- "Create a dashboard for healthcare analytics"
- "Design a portfolio website with dark mode"
- "Generate a design system for a fintech banking app"

If the skill is installed correctly, AutoClaw will:

1. Recognize the design intent from your prompt
2. Load the `ui-ux-pro-max` skill body
3. Emit a complete design system (colors, typography, spacing, etc.)
4. Recommend a UI style, chart types, and stack-specific implementation hints

If nothing happens, check **Settings → Skills** in AutoClaw - the skill must show as enabled.

---

## Troubleshooting

### Skill doesn't appear in Settings → Skills

- Confirm the file path: `~/.autoclaw/skills/ui-ux-pro-max/SKILL.md` (must be exactly this layout)
- Confirm the directory name matches the `name` field in the frontmatter (both should be `ui-ux-pro-max`)
- Click **Refresh** in the Skills page
- Check the diagnostics panel for any error messages

### Skill appears but never triggers automatically

Common causes (from the ZCode/AutoClaw docs):

1. **Too many skills enabled** - AutoClaw injects metadata for every enabled skill into the model context. If you've blown past the budget, descriptions get dropped and the model can't decide when to trigger. Disable skills you don't actively use.
2. **Vague description** - The skill's `description` already spells out triggers, but if you've customized it, make sure your version includes concrete trigger phrases.
3. **Inside a subagent with a custom `tools` allowlist** - If you've defined a subagent that restricts tools, the skill tool may not be on the allowlist. Remove the `tools` field or add the skill tool explicitly.

### Search script fails to run

```bash
# Verify Python 3 is on PATH
python3 --version

# Run the script directly to see the error
python3 ~/.autoclaw/skills/ui-ux-pro-max/scripts/search.py "saas dashboard" --design-system
```

The script uses only the Python 3 standard library, so there should be no missing-module errors. If you see import errors, your Python install may be non-standard.

---

## Updating

To pull the latest changes from upstream:

```bash
cd ~/.autoclaw/skills/ui-ux-pro-max
git pull origin main
```

Then refresh AutoClaw's skill list.

---

## Uninstall

```bash
rm -rf ~/.autoclaw/skills/ui-ux-pro-max
```

Then refresh AutoClaw's skill list to confirm it's gone.

---

## License

Same as upstream: MIT.
