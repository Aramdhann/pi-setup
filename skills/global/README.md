# Global skills

Snapshot of standalone skills installed under `~/.agents/skills/`.

| Skill | Purpose | Restore destination |
| --- | --- | --- |
| `find-skills` | Discover and install skills from the skills ecosystem | `~/.agents/skills/find-skills/` |
| `migrate-radix-to-base` | Migrate React components from Radix UI to Base UI | `~/.agents/skills/migrate-radix-to-base/` |
| `shadcn` | Manage shadcn components, registries, styling, and design systems | `~/.agents/skills/shadcn/` |

Each skill lives in the matching subdirectory here. These are local installed-file snapshots, not verified upstream revisions; upstream commits and licenses were not independently established for these three skills. Supporting references, scripts, and assets are preserved unchanged. Review scripts before executing them.

`context7-mcp` and `karpathy-guidelines` are already maintained as top-level snapshots in `skills/`; do not duplicate them here.

Package-provided skills, including Ponytail and anti-slop, stay package-managed and are not copied here. This avoids duplicate skill names and stale package copies.

## Update

Compare current global files with this directory, then copy only reviewed changes. Preview each `rsync` command with `--dry-run` first: `--delete` removes snapshot files absent from the local source. These commands require rsync and must be run from the repository root. For a full refresh:

```bash
rsync -a --delete ~/.agents/skills/find-skills/ skills/global/find-skills/
rsync -a --delete ~/.agents/skills/migrate-radix-to-base/ skills/global/migrate-radix-to-base/
rsync -a --delete ~/.agents/skills/shadcn/ skills/global/shadcn/
git diff --check
```

Review `git diff` before committing. Never copy credentials, caches, logs, or unrelated files.
