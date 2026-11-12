---
event_date: "2026-11-11"
recorded_at: "2026-11-11 14:51:00"
domain: "interview"
type: "component"
client: "codex"
author: "agent"
agent_role: "cli_skill_auditor"
status: "active"
superseded_by: null
---

# Playbook & Pitfalls (实战范式与高频避坑说明书)

Use this reference when tailoring or deriving a resume from an existing base
resume, cleaning up messy imported structure, fitting content onto one page, or
recovering from errors and human rejections.

---

## 1. Golden Playbook: Derive a 1-page demo from an existing resume (`v8`)

When the user asks you to generate a role-specific cut or previewable 1-page
demo from an existing resume (e.g. `v8`), follow this exact 7-step loop:

### Step 1 — Check environment and resumable auth state
```bash
"$SKILL_DIR/folio" doctor
"$SKILL_DIR/folio" auth status
```
- If `state` is `"not_connected"` or `"awaiting_browser_approval"`, follow the
  returned `nextCommand` (`auth login --no-wait` or `auth complete --no-wait`).
  Never poll in a loop or approve on the user's behalf.

### Step 2 — Locate and inspect the source resume in ascending cost
```bash
"$SKILL_DIR/folio" resume list
"$SKILL_DIR/folio" resume outline <sourceId>
"$SKILL_DIR/folio" resume get <sourceId>
```
- `list` gives summaries (`characterCount`, `photo.bytes`, `derivedFrom`,
  `pendingProposalCount`) **plus** account-wide `quota` (`used`, `limit: 10`,
  `remaining`, `active`, `archived`, accurate even without `--archived`).
- `outline` gives section/entry IDs, headings, dates and character distribution.
- `get` gives the prose without Base64 photo bytes, plus `lastChange`
  (`actorType: "human" | "agent"`, `reason`, `version`).

### Step 3 — Derive with `resume copy` (NEVER `resume create`)
```bash
"$SKILL_DIR/folio" resume copy <sourceId> --id <demoId> \
  --title "<approved demo title>" --version <latestSourceVersion> --dry-run
"$SKILL_DIR/folio" resume copy <sourceId> --id <demoId> \
  --title "<approved demo title>" --version <latestSourceVersion>
```
- **Why**: `resume copy` seeds `<demoId>` v1 with the source resume's saved
  content/style, records `derivedFrom: { resumeId, version }`, and automatically
  inherits the source resume's `brief`. Pass `--dry-run` first whenever you want
  to verify `quota.remaining > 0` and `expectedSourceVersion` without consuming
  a slot.
- Using `resume create` instead leaves `<demoId>` as a blank v1 draft until the
  human accepts your Proposal, and makes `proposal diff` show everything as
  `added` instead of showing what you removed or kept from `<sourceId>`.

### Step 4 — Read and update `brief` + `proposal decisions`
```bash
"$SKILL_DIR/folio" brief get <demoId>
"$SKILL_DIR/folio" proposal decisions <demoId>
"$SKILL_DIR/folio" brief update --file "$brief_file"
```
- Record durable user constraints (`goal`, `audience`, `constraints`,
  `mustKeep`, `mustDrop`, `questions`) in the brief so future sessions inherit
  them.
- Check `proposal decisions <demoId>` for any previously rejected batches and
  their human `reason`. Never re-submit a rejected batch unchanged.

### Step 5 — Dry-run layout BEFORE submitting (`layout diagnose --file`)
```bash
"$SKILL_DIR/folio" layout diagnose <demoId> --file "$draft_doc_file"
```
- `--file` accepts either a `document` object (`{ title, language, sections,
  style, target, photo? }`) or the `{ resume: ... }` wrapper returned by
  `resume get`.
- Check the returned `source: "document"`:
  - If `onePage.fits` is `true` and `current.pages > 1`, apply
    `onePage.recommendedStyle` to your draft's `style` and re-run `--file` to
    confirm `current.pages === 1`.
  - If `onePage.fits` is `false`, inspect `current.usedHeight`,
    `current.availableHeight`, `onePage.estimatedOverflowPx`,
    `onePage.suggestedReductionLines`, `onePage.schemaFloorProbe`, and
    `sectionUsage`:
    - `onePage.readabilityFloor` (`fontSize: 10`, `lineHeight: 1.35`,
      `sectionGap: 10`, `pageMargin: 30`) is Folio's automatic print-readability
      floor — `recommendedStyle` will not auto-shrink below it.
    - `onePage.schemaFloorProbe` shows what happens at the absolute schema floor
      (`fontSize: 9.75`, `lineHeight: 1.2`, `sectionGap: 6`, `pageMargin: 22.7`).
      Prefer trimming low-relevance bullets or switching to a higher-capacity
      template before pushing typography below `readabilityFloor`.
- **Single-page template pixel capacity (`availableHeight`)**:
  - `cn-classic` (Chinese serif, A4): **`808.87px`** — highest capacity (+48.87px over `onyx`)
  - `onyx` / `rhyhorn` / `bronzor` / `persian` (A4 canvas): **`760px`**
  - `en-classic` (English sans-serif, Letter): **`695.3px`**
  - `en-engineering` (English dense serif, Letter): **`684.71px`**

### Step 6 — Submit a self-explaining whole-batch Proposal
```bash
"$SKILL_DIR/folio" action validate proposal.submit --file "$proposal_file"
"$SKILL_DIR/folio" proposal submit --file "$proposal_file"
```
- Always populate `notes.rationale` (one entry per removed/changed section or
  entry, e.g. `{"target": "entry:exp_3", "reason": "..."}`), `notes.tradeoffs`,
  `notes.rejected` and `notes.openQuestions`. The reviewer sees the review
  screen without your chat history.

### Step 7 — Verify diff & rendered preview, then hand off `url`
```bash
"$SKILL_DIR/folio" proposal diff <proposalId>
"$SKILL_DIR/folio" layout diagnose <demoId> --proposal <proposalId>
"$SKILL_DIR/folio" proposal preview <proposalId>
```
- Verify `proposal diff`: confirm `groups.removed` lists only the entries you
  intended to cut and each carries its `reason`.
- Verify `proposal preview`: confirm `pages`, `onePageFits` and
  `layoutUnsafe: false`, then **send the returned `url` directly to the user**.

---

## 2. High-Frequency Pitfalls (常见踩坑与正确做法对照表)

| Pitfall scenario | Wrong move (anti-pattern) | Correct playbook |
| :--- | :--- | :--- |
| **1. Deriving a new cut/demo from an existing resume** | Running `resume create` (blank v1) and submitting `replace_document` on top of it. User sees an empty draft in the editor and `proposal diff` cannot show what was cut from the source resume. | Run `resume copy <sourceId> --id <newId> --version <v>`, then submit your Proposal against `<newId>`. |
| **2. Finding which resume to edit** | Calling `resume get --photo` across multiple resumes (a single portrait is ~400KB Base64 / ~110k tokens and truncates context). | Call `resume list` ➔ `resume outline <id>` ➔ `resume get <id>` (omit `--photo` unless explicitly inspecting the portrait). |
| **3. Imported resume has merged entries or stray contact lines** | Editing prose inside a single giant merged entry, or leaving experiences inside `"其他原文内容"`. | Use `replace_document` (`factBasis: "user_provided"`, `polishPolicy: "no_new_facts"`) to split distinct companies/projects into separate `entries[]` with their own `heading`, `subtitle`, `date` and `body`, and explain the structural cleanup in `notes.rationale`. |
| **4. Guessing 1-page fit by character count** | Estimating page count from character count and submitting blindly, or describing layout in prose. | Dry-run `layout diagnose <id> --file <doc.json>` before submitting; run `proposal preview <proposalId>` after submitting and share the preview `url`. |
| **5. Handling human rejection or mid-flight human edits** | Re-submitting the same Proposal payload or ignoring why the user rejected the last batch. | Read `proposal decisions <id>` (check `reason`) and `history changes <id> --since <oldVersion>`; address the feedback and use `supersedesProposalId` if replacing a still-pending batch. |
| **6. Saving a target JD alongside content edits** | Leaving a user-provided JD only in chat history, or calling `target update` *after* `proposal submit` (which bumps `resume.version` and marks the pending Proposal stale). | Check `target.hasJd` in `resume list` / `resume outline` and read existing JD with `target get <id>`. When saving a new JD, run `target update <id> --role "..." --company "..." --jd-file <jd.txt>` **before** `proposal submit`, then use the returned `version` as `baseVersion`. |

---

## 3. Error Code & Edge-Case Recovery

| Code / Signal | Meaning | Deterministic recovery |
| :--- | :--- | :--- |
| `STALE_VERSION` | Resume or brief moved to a newer version since you read it. | Re-read `resume get <id>` + `history changes <id> --since <v>` (or `brief get <id>`). Re-evaluate your changes on the new baseline; submit with a new `proposalId` / `idempotencyKey`. |
| `LAYOUT_OVERFLOW` (422) or `layoutUnsafe: true` | A single unsplittable DOM unit (e.g. one giant `<li>` bullet item, or a huge paragraph in a non-splitting template) is taller than an entire page (`current.unsplittableEntries` names the offending entry IDs). | Break the oversized bullet/paragraph into smaller `<li>` items or shorter paragraphs, then re-run `layout diagnose --file`. |
| `CONFIG_UNWRITABLE` | `FOLIO_CONFIG_DIR` or `config.json` cannot be written in the current sandbox; CLI halted *before* consuming the single-use `deviceCode`. | Fix `FOLIO_CONFIG_DIR` permissions/path (e.g. point to a writable profile dir with the pending config) and re-run `folio auth complete --no-wait` — no browser re-approval needed. |
| `DEVICE_CONSUMED` | The single-use `deviceCode` was already exchanged by an earlier/parallel process and no new local token is stored; stale `pending` state was cleared. | Follow `details.nextCommand`: run `folio auth login --no-wait` to start a fresh pairing code. |
| `QUOTA_EXCEEDED` (422) | Account already holds `10/10` active + archived resumes (`quota.remaining === 0`). | Check `resume list --archived`; ask the user whether to archive/move an old draft to trash in Folio UI before creating or copying. |
| `STATE_CONFLICT` on `proposal preview` | The proposal was already accepted, rejected, or superseded (`status !== "pending"`). | Preview links only exist for pending batches. Run `proposal get <id>` or `resume get <resumeId>` to inspect the decided state. |
| `ACTION_NOT_AUTHORIZED` on `brief update` | The connection was paired before `brief:write` existed or without that scope. | Tell the user clearly that re-pairing is needed to persist the brief; continue the task without silently dropping their constraints. |
| `ACCOUNT_MISMATCH` / `PROFILE_ORIGIN_MISMATCH` | Current `FOLIO_CONFIG_DIR` belongs to a different account or server origin. | Stop immediately. Use a separate `FOLIO_CONFIG_DIR` profile; never overwrite or logout existing credentials without explicit user consent. |
