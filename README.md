# Personal Pi setup

Portable snapshot of this laptop's **global Pi configuration**. Restore instructions: [SETUP.md](SETUP.md).

This is a configuration backup, **not an installable Pi package**. Clone it and follow SETUP.md; do not run `pi install` on this repository.

## Contents

- `config/pi-agent/settings.json`: existing display/model preferences; eight package sources pinned to installed revisions.
- `config/pi-agent/extensions/pi-rtk-optimizer/config.json`: current RTK extension options.
- `config/pi-agent/pi-droid-styling.json`: saved styling preferences (no matching standalone styling extension was found; may be inactive).
- `config/pi-agent/models.example.json`: custom 9router model definitions, with endpoint and credential replaced by placeholders.
- `skills/context7-mcp/SKILL.md`: manually installed documentation-lookup guidance.
- `skills/karpathy-guidelines/SKILL.md`: upstream skill from https://github.com/multica-ai/andrej-karpathy-skills at commit `2c606141936f1eeef17fa3043a72095b4765b9c2` (declares MIT license).

Ponytail and other package-provided skills/extensions come from pinned packages, not copied dependency trees. No custom global prompt templates, custom themes, or standalone extension source files were found. No global `AGENTS.md`, `SYSTEM.md`, `APPEND_SYSTEM.md`, or keybindings file was found in the inspected Pi locations.

## Installed inventory

| Resource | Captured version / revision |
| --- | --- |
| Pi (`@earendil-works/pi-coding-agent`) | `1.4.0` |
| Node.js | `22.22.0` (Pi requires >= `22.19.0`) |
| npm | `10.9.4` |
| RTK binary | `0.46.0` |
| Ponytail | `09c434ab73ca43761c5d8b0c5d341926c2fba710` |
| pi-rtk-optimizer | `d155d253cb2f1358e34e717d47a82ebccb08cb8e` |
| @ff-labs/pi-fff | `0.11.0` |
| @upstash/context7-pi | `0.1.3` |
| @narumitw/pi-usage | `0.68.3` |
| billion-context | `0.1.208` |
| pi-web-access | `0.45.0` |
| pi-cc-extensions | `3d0c7195094f9441c3adcbd0c4681c5491932bd3` |

Source preferences used unpinned package names; backup pins them to make restoration more repeatable. Live laptop settings were not changed. Git package changes were confined to generated lockfiles; these were not copied.

## New packages

- **pi-web-access** adds `web_search`, `source_check`, `fetch_content`, and `get_search_content` for web research and URL/PDF/video/GitHub content retrieval. Keyless search is available through Exa MCP; optional provider credentials belong in private local configuration, never this repository.
- **pi-cc-extensions** adds Claude Code-style tool output, rich diffs, a custom footer, `/ccstyle` settings, `/context` inspection, session references, and bundled CC Dark/CC Light themes. It can display quota information from `@narumitw/pi-usage`.

Neither `~/.pi/agent/web-search.json` nor `~/.pi/agent/pi-cc-extensions.json` existed when these packages were captured; no package-specific preferences were copied. Configure them locally if needed. The older `pi-droid-styling.json` remains a separate, potentially inactive preference file.

## Security and limits

Never commit credentials, `auth.json`, real `models.json`, `models-store.json`, shell profiles, private endpoints, session transcripts, logs, caches, or databases. `.gitignore` is a guardrail, not a secret scanner. Review every update before pushing, including Markdown and JSON files.

This is a manual snapshot, not automatic synchronization. After changing Pi setup, update relevant files and package pins deliberately, review diff, then commit and push. Project-local Pi settings, external MCP client configs, provider server deployments, shell setup, and machine-wide tools are outside this backup. Configure provider authentication separately on each device.

Skills are behavioral guidance, not enforcement. Installing Ponytail does not prove it will be active automatically in every future session; inspect startup state or explicitly invoke `/skill:ponytail`. Karpathy is available through `/skill:karpathy-guidelines`.
