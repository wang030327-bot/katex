# KaTeX plugin for ChatGPT and Codex

This repository is a marketplace source for the `katex` plugin. The plugin bundles a reusable skill for integrating, configuring, troubleshooting, and securing KaTeX math rendering in browser, Node.js, SSR, static-site, and component-framework projects.

## Install in a ChatGPT workspace

Workspace administrators can import and sync this GitHub marketplace:

```text
https://github.com/wang030327-bot/katex
```

After the marketplace is available in the workspace:

1. Open **Plugins**.
2. Find **KaTeX** and select the plus button to install it.
3. Start a new chat or Work session.
4. Invoke the plugin or its bundled skill with `@katex`.

Plugin availability remains subject to workspace administrator policy.

## Install in Codex CLI from a local checkout

```bash
git clone https://github.com/wang030327-bot/katex.git
codex plugin marketplace add /absolute/path/to/katex
codex plugin add katex@katex-marketplace
```

Start a new Codex session after installation, then invoke the skill with `$katex`.

## Contents

- `.agents/plugins/marketplace.json` — marketplace catalog
- `plugins/katex/.codex-plugin/plugin.json` — plugin manifest
- `plugins/katex/skills/katex/SKILL.md` — KaTeX skill instructions

This project is an independent Codex/ChatGPT plugin and is not the official KaTeX project.
