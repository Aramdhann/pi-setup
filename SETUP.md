# Restore on another laptop

Give your AI this request:

> Read README.md and SETUP.md. Restore this global Pi setup. Inspect existing configuration first, back up files before changing them, preserve unrelated settings, and ask before replacing conflicting choices. Never read credentials into chat or commit them. Install pinned packages, restore skills and preferences, help configure provider authentication locally, and report verification results and anything missing.

## 1. Install prerequisites and clone

Install Git and Node.js >= 22.19.0 with npm. Captured Node version: 22.22.0.

```bash
npm install -g --ignore-scripts @earendil-works/pi-coding-agent@1.4.0
mkdir -p ~/code_personal
git clone https://github.com/Aramdhann/pi-setup.git ~/code_personal/pi-setup
cd ~/code_personal/pi-setup
pi --version
```

If `pi` already exists, inspect its version and installation method first; do not install a second competing copy blindly. GitHub authentication may be required to clone/push.

Install RTK separately from https://github.com/rtk-ai/rtk if command rewriting is wanted (captured version: 0.46.0). Ensure `rtk` is on PATH and run `rtk --version`. Extension is configured to fall back to original commands when RTK is absent.

## 2. Restore preferences and manual skills

Destination mapping:

| Repository path | Destination |
| --- | --- |
| `config/pi-agent/settings.json` | `~/.pi/agent/settings.json` |
| `config/pi-agent/extensions/pi-rtk-optimizer/config.json` | `~/.pi/agent/extensions/pi-rtk-optimizer/config.json` |
| `config/pi-agent/pi-droid-styling.json` | `~/.pi/agent/pi-droid-styling.json` |
| `skills/context7-mcp/` | `~/.agents/skills/context7-mcp/` |
| `skills/karpathy-guidelines/` | `~/.agents/skills/karpathy-guidelines/` |

On a fresh installation, create parent directories and copy files to these destinations. On an existing installation, make timestamped local backups first and merge settings instead of overwriting. Preserve existing packages; avoid installing both pinned and unpinned declarations of the same package. Compare existing skill directories before replacement. Do not invent missing prompt templates or custom extensions.

## 3. Install package dependencies

After restoring or merging settings, reconcile the package declarations:

```bash
pi update --extensions
pi list
```

On an existing setup, review all configured package entries before running this command: it reconciles more than just this backup's entries. Pinned revisions should remain pinned. Package installation executes third-party code; inspect sources before trusting them. Native binaries used by packages may require platform-specific setup; inspect startup warnings rather than copying binaries from the old laptop.

Only use the captured `npm:billion-context@0.1.204` entry for this setup. Do not additionally install the legacy `billion-context-pi` extension: native and legacy integrations can double-compress context.

Context7 works without a key at IP-based limits. For higher quotas, configure `CONTEXT7_API_KEY` privately in your shell/secret manager; never put its value in this repository.

## 4. Configure model access privately

Current preferences select `9router/work-daily-used`; without a working provider, that default cannot serve requests.

1. Copy `config/pi-agent/models.example.json` to `~/.pi/agent/models.json` only if no existing file would be overwritten; otherwise merge provider definitions after backup.
2. Edit the copied file locally. Replace `https://YOUR-9ROUTER-HOST.example/v1` with your actual OpenAI-compatible endpoint.
3. Keep `apiKey` as `${NINE_ROUTER_API_KEY}` and supply that environment variable privately before launching Pi. Do not paste credentials into AI chat or commit them.
4. Your provider must expose the captured aliases: `work-daily-used`, `long-context`, `review`, and `cheap-fast`. This repo does not deploy 9router or restore its routing/account data.

Alternatively, use Pi `/login` and `/model` to choose another provider, then save that model as default. Do not copy `auth.json` or `models-store.json` into Git. If default 9router is unavailable, change `defaultProvider`/`defaultModel` to a configured provider/model before relying on agent requests.

## 5. Verify interactively

- Run `pi list`: confirm six captured packages or explain intentional differences.
- Start Pi and inspect startup diagnostics for extension errors and duplicate skill names.
- Run `/reload`; confirm `/skill:ponytail` and `/skill:karpathy-guidelines` resolve.
- Ask a library-documentation question; confirm `resolve-library-id` and `query-docs` work.
- Run `/rtk show` and `/rtk verify` to check effective config and RTK availability.
- Confirm `fffind` and `ffgrep` tools are available.
- Confirm billion-context compression/delegate tools load. Test a small read-only delegate with a configured provider; package installation alone does not guarantee delegate authentication.
- Use `/model` and a small request to verify provider access; never print keys while debugging.

No full new-device restore was performed when this snapshot was created. Availability of third-party packages, platform-specific binaries, providers, and interactive extension behavior must be verified on the target laptop.
