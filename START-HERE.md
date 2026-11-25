---
event_date: "2026-11-08"
recorded_at: "2026-11-08 22:51:48"
domain: "interview"
type: "component"
client: "codex"
author: "agent"
agent_role: "cli_skill_auditor"
status: "active"
superseded_by: null
---

# Install / start

## Public install source

Users can clone the standalone Skill repository (it ships the built CLI, so no
local Folio checkout is needed):

```bash
DEST="${CODEX_HOME:-$HOME/.codex}/skills/folio-resume-workbench"  # use that Agent's real skills dir
git clone https://github.com/suntianchong/folio-resume-skill "$DEST"
chmod +x "$DEST/folio"
```

No global install is required: give your Agent the absolute
`<FOLIO_PROJECT_ROOT>/apps/folio-cli/skill/SKILL.md` path, or use Folio's
“账户 → 外部 Agent 配置” current handoff message. Read the Skill, not every
reference. Requires Node 24+, built CLI and a user-started Folio service.

## Optional copy into an Agent skill directory

**Only after user authorizes installation.** Choose that Agent's actual skill
directory (Codex example: `${CODEX_HOME:-$HOME/.codex}/skills`). Do not replace an
existing installation without approval; do not run `curl|sh` or install software.
Copy the **whole directory**, retaining wrapper, references and both task cards:

```bash
export FOLIO_PROJECT_ROOT="/absolute/path/to/folio"
DEST="${CODEX_HOME:-$HOME/.codex}/skills/folio-resume-workbench"
test ! -e "$DEST" || { echo "已有Skill；请先确认如何更新" >&2; exit 1; }
mkdir -p "$(dirname "$DEST")"
cp -R "$FOLIO_PROJECT_ROOT/apps/folio-cli/skill" "$DEST"
chmod +x "$DEST/folio"
```

Supply `FOLIO_PROJECT_ROOT` to **each terminal invocation / Agent environment**;
a one-off shell export is not guaranteed to persist in another tool or chat.
Alternatively set `FOLIO_CLI_BIN="/absolute/path/to/already/installed/folio"`
(actual CLI, not another Skill wrapper); PATH-installed `folio` is also found.
The wrapper never assumes a copied directory's parents are Folio and never
changes caller cwd. `FOLIO_NODE` can select an executable Node 24+ path.
It does not auto-build; maintainer command if needed:
`node apps/folio-cli/scripts/build.mjs` from the Folio project.

## Shortest connection message

```text
请读取并严格遵守 Folio Skill：https://github.com/suntianchong/folio-resume-skill
若这台电脑上还没有 Skill，先经我同意再 clone 该仓库到你的 skills 目录，
chmod +x folio；仓库自带 bin/folio.mjs，不需要 Folio 源码或 npm 安装。
FOLIO_ORIGIN=<the API address shown by my Folio>
FOLIO_CONFIG_DIR=<separate absolute profile directory for this client/environment>
预期账号：<my confirmed Folio email>。
先doctor/auth status；未连接则auth login --email该邮箱 --no-wait，
给我verificationUri后停在人工批准处，不要索要Token或代批准。
我确认后再auth complete --no-wait并核对身份。先不要创建或修改简历。
```

`FOLIO_ORIGIN` may be omitted for an already-configured profile; wrapper honors
its saved origin. Don't reuse a profile across services/accounts; mismatch fails
without sending/storing that profile's old token in the new service. `--replace`
only with user's explicit credential-replacement approval.

Then give the user-selected resume ID and task. Full fictional rewrite/portrait
card remains at [COMPLETE-RESUME-TASK.md](COMPLETE-RESUME-TASK.md). The Agent
submits pending batches, while **you** preview and accept/reject/export in Folio.
