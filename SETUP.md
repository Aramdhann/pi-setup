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
| `skills/global/find-skills/` | `~/.agents/skills/find-skills/` |
| `skills/global/migrate-radix-to-base/` | `~/.agents/skills/migrate-radix-to-base/` |
| `skills/global/shadcn/` | `~/.agents/skills/shadcn/` |

On a fresh installation, create parent directories and copy files to these destinations. On an existing installation, make timestamped local backups first and merge settings instead of overwriting. Preserve existing packages; avoid installing both pinned and unpinned declarations of the same package. Compare existing skill directories before replacement. Copy each skill directory recursively, including its references, scripts, and assets; see [`skills/global/README.md`](skills/global/README.md) for the standalone-skill inventory and refresh steps. Do not copy package-provided skills into `~/.agents/skills/`, since duplicate names can cause load warnings. Do not invent missing prompt templates or custom extensions.

## 3. Install package dependencies

After restoring or merging settings, reconcile the package declarations:

```bash
pi update --extensions
pi list
```

On an existing setup, review all configured package entries before running this command: it reconciles more than just this backup's entries. Pinned revisions should remain pinned. Package installation executes third-party code; inspect sources before trusting them. Native binaries used by packages may require platform-specific setup; inspect startup warnings rather than copying binaries from the old laptop.

Only use the captured `npm:billion-context@0.1.208` entry for this setup. Do not additionally install the legacy `billion-context-pi` extension: native and legacy integrations can double-compress context.

Context7 works without a key at IP-based limits. For higher quotas, configure `CONTEXT7_API_KEY` privately in your shell/secret manager; never put its value in this repository.

The snapshot also includes `pi-web-access`, `pi-cc-extensions`, `cc-safety-net`, and `anti-slop`:

- Web access supports keyless search through Exa MCP. Optional provider keys and endpoints belong in private `~/.pi/agent/web-search.json` or environment variables; do not commit them. Browser-cookie access is opt-in; leave it disabled unless explicitly needed.
- CC styling uses `/ccstyle` and optional `~/.pi/agent/pi-cc-extensions.json`. No preferences file was captured, so package defaults apply. Bundled CC Dark/CC Light themes can be selected through `/settings`; the snapshot keeps the existing `dark` theme.
- Safety Net loads through the pinned Pi package; no separate all-CLI installer is needed for this restore. Use `/cc-safety-net` for help. No custom policy is captured; review local policy separately. It checks tool calls, but is not a sandbox or a replacement for backups and least-privilege credentials.

- anti-slop adds six Pi skills. They are restored with the package; start a new Pi session or run `/reload`, then confirm `/skill:antislop` resolves. Invoke `/skill:antislop` when UI, copy, accessibility, responsive layout, or code-comment work needs its guidance. No global antislop mode preference was captured; UI work may prompt for a mode unless you set one explicitly.

## 4. Configure model access privately

Current preferences select `9router/work-daily-used`; without a working provider, that default cannot serve requests.

1. Copy `config/pi-agent/models.example.json` to `~/.pi/agent/models.json` only if no existing file would be overwritten; otherwise merge provider definitions after backup.
2. Edit the copied file locally. Replace `https://YOUR-9ROUTER-HOST.example/v1` with your actual OpenAI-compatible endpoint.
3. Keep `apiKey` as `${NINE_ROUTER_API_KEY}` and supply that environment variable privately before launching Pi. Do not paste credentials into AI chat or commit them.
4. Your provider must expose the captured aliases: `work-daily-used`, `long-context`, `review`, and `cheap-fast`. This repo does not deploy 9router or restore its routing/account data.

Alternatively, use Pi `/login` and `/model` to choose another provider, then save that model as default. Do not copy `auth.json` or `models-store.json` into Git. If default 9router is unavailable, change `defaultProvider`/`defaultModel` to a configured provider/model before relying on agent requests.

## 5. Verify interactively

- Run `pi list`: confirm ten captured packages or explain intentional differences.
- Start Pi and inspect startup diagnostics for extension errors and duplicate skill names.
- Run `/reload`; confirm `/skill:ponytail`, `/skill:karpathy-guidelines`, `/skill:find-skills`, `/skill:migrate-radix-to-base`, and `/skill:shadcn` resolve without duplicate-name warnings.
- Ask a library-documentation question; confirm `resolve-library-id` and `query-docs` work.
- Run `/rtk show` and `/rtk verify` to check effective config and RTK availability.
- Confirm `fffind` and `ffgrep` tools are available.
- Confirm `web_search`, `source_check`, `fetch_content`, and `get_search_content` load; try a public web search and URL fetch to verify network/provider access.
- Run `/ccstyle` and `/context`; confirm CC styling loads without renderer/footer conflicts with other extensions.
- Confirm `/skill:antislop` and the relevant `antislop-*` skills resolve after reload.
- Confirm Safety Net loads without extension errors and `/cc-safety-net` resolves. Verify blocking only in a disposable test repository; never test destructive commands against real work or probe real secrets.
- Confirm billion-context compression/delegate tools load. Test a small read-only delegate with a configured provider; package installation alone does not guarantee delegate authentication.
- Use `/model` and a small request to verify provider access; never print keys while debugging.

No full new-device restore was performed when this snapshot was created. Availability of third-party packages, platform-specific binaries, providers, and interactive extension behavior must be verified on the target laptop.
