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

# Task commands

Run through `"$SKILL_DIR/folio"`. Errors are JSON on stderr and exit 1;
normal successes are JSON on stdout and exit 0. No-argument/help prints help,
unknown commands/flags fail. `--file` is relative to **caller cwd**, or absolute;
submit/validate/run can also consume JSON stdin. Do not send tokens to chat.

## Read

```bash
"$SKILL_DIR/folio" resume list --archived
"$SKILL_DIR/folio" resume outline <resumeId>
"$SKILL_DIR/folio" resume get <resumeId>          # add --photo only for portrait questions
"$SKILL_DIR/folio" history list <resumeId>
"$SKILL_DIR/folio" history changes <resumeId> --since <oldVersion>
"$SKILL_DIR/folio" proposal list <resumeId>
"$SKILL_DIR/folio" proposal get <proposalId>
"$SKILL_DIR/folio" proposal diff <proposalId>
"$SKILL_DIR/folio" proposal decisions <resumeId>
"$SKILL_DIR/folio" brief get <resumeId>
```

Read in ascending cost: `list` is summaries (counts, target, photo size, pending
proposals), `outline` is structure without prose, `get` is the document without
image bytes. Never inline base64 into the prompt.
List defaults to active only; `--archived` includes archived, never trash.
History is saved snapshots/audited changes, not original attachments.

## New independent draft

```bash
"$SKILL_DIR/folio" resume create --id <newId> --title "<approved title>" \
  --language zh --idempotency-key <stableKey>
"$SKILL_DIR/folio" resume get <sourceId>
"$SKILL_DIR/folio" resume copy <sourceId> --id <newId> --title "<approved title>" \
  --version <latestSourceVersion> --idempotency-key <stableKey>
```

Create is blank (use only when starting from scratch with no source resume in
Folio); copy preserves source saved content/style and `derivedFrom`. When
deriving a new resume or demo from an existing resume, always use `resume copy`
followed by a Proposal on `<newId>` so the new resume has no blank intermediate
state and `proposal diff` shows all removed/changed/kept items against the source.
Ten-resume quota includes archived. Source stays unchanged. Retry the same
creation intent with the same ID/key; do not claim arbitrary key-based deduplication:
current create/copy return an existing target by ID before checking source version.

## Existing resume: whole-batch Proposal

Read latest resume and stable section/entry IDs. Supported `changes[].op`:
`update_entry`, `update_style`, `reorder_section`, `set_section_hidden`,
`replace_document` (sole change; see complete-resume reference).

```json
{
  "proposalId": "proposal_example_v1",
  "idempotencyKey": "example-v1",
  "resumeId": "user_selected_id",
  "baseVersion": 1,
  "intent": "仅修改用户指定的强调色，不新增事实",
  "changes": [{"op": "update_style", "patch": {"accentColor": "#526150"}}],
  "notes": {
    "rationale": [{"target": "style:accentColor", "reason": "用户指定了这个颜色"}],
    "tradeoffs": ["强调色变浅后，打印时对比度略降"],
    "rejected": [{"option": "换成品牌蓝", "reason": "用户没有提供品牌色"}],
    "openQuestions": ["是否需要同时调整模板？"]
  }
}
```

`notes` is how the proposal explains itself to the reviewer and to the next
Agent. `rationale[].target` must be one of `document`, `style`, `photo`,
`target`, `order`, `section:<id>` or `entry:<id>`; put one entry per item you
actually touched. Use `tradeoffs` for what the user gives up, `rejected` for
options you dropped and why, `openQuestions` for anything you could not decide
from the user's material. Omit `notes` only for a trivially reversible edit.

Use a private temporary file (not the repo), then:

```bash
"$SKILL_DIR/folio" action schema proposal.submit
"$SKILL_DIR/folio" action validate proposal.submit --file "$proposal_file"
"$SKILL_DIR/folio" proposal submit --file "$proposal_file"
"$SKILL_DIR/folio" proposal diff <proposalId>
"$SKILL_DIR/folio" proposal preview <proposalId>
"$SKILL_DIR/folio" proposal get <proposalId>
"$SKILL_DIR/folio" resume get <resumeId>
```

`proposal diff` is the reviewer's view of your batch: `removed` is what
disappears when the user accepts, `added`/`changed` are excerpts, `kept` is what
survives. Read it before you report success — if `removed` contains something
you did not intend to delete, fix the batch and resubmit with
`supersedesProposalId` instead of explaining it in chat.

`proposal preview` renders the batch the way the user will actually receive it:
the real export renderer at the real page size. It answers "does this fit and how
much whitespace is left" without guessing:

```json
{
  "pages": 2, "onePageFits": false, "layoutUnsafe": false,
  "url": "https://<folio>/api/preview/<token>",
  "pdfUrl": "https://<folio>/api/preview/<token>/document.pdf",
  "pageImageUrls": ["https://<folio>/api/preview/<token>/page-1.png", "..."],
  "expiresAt": "2026-11-10T09:30:00.000Z"
}
```

Pass `url` to the user as a clickable link instead of describing the layout.
The link is readable without a login (it is meant for chat), so treat it as a
secret: it expires in 30 minutes and dies the moment the batch is accepted or
rejected. `layoutUnsafe: true` means one block cannot be split across pages, the
export would be refused, and `pdfUrl` is null — shorten that entry and resubmit;
do not tell the user it "almost fits". A non-pending proposal cannot be
previewed at all (`STATE_CONFLICT`): re-read the batch instead.

Local validation mirrors current input schemas, including nested fields,
photo declarations and sole replacement. Server submission remains authoritative
for schemas, account, capabilities and version. Neither validation establishes
that user-provided facts are true; you must map
each factual phrase to the user's material. Do not claim local validation proves
all target IDs exist, all changes can be applied, or real preview/PDF acceptance.
Report pending vs accepted exactly; only the human reviews/applies the batch.

On stale, read latest + changes; recompute, then use a new proposal ID/key with
`supersedesProposalId` for a pending old batch. Retrying a previously successful
key may return its original Proposal, even if it is now stale or superseded.

## Brief and decisions (read before planning)

```bash
"$SKILL_DIR/folio" brief get <resumeId>
"$SKILL_DIR/folio" proposal decisions <resumeId>
"$SKILL_DIR/folio" brief update --file "$brief_file"   # or stdin
```

```json
{
  "resumeId": "user_selected_id",
  "expectedBriefVersion": 0,
  "goal": "六周内拿到资深产品设计师的面试",
  "audience": "招聘方与用人经理",
  "constraints": ["必须一页", "不写期望薪资"],
  "mustKeep": ["设计系统项目"],
  "mustDrop": ["与岗位无关的证书"],
  "questions": [{"id": "q1", "question": "是否写管理人数？", "status": "open"}]
}
```

The brief is the durable context: goal, audience, constraints, must-keep,
must-drop and open questions. It never enters the document or the PDF. Send only
the fields you are changing; the rest are preserved. `expectedBriefVersion` is
the version you read — a mismatch is `STALE_VERSION`, so re-read rather than
retry.

Update it when the user tells you something durable: a new goal, a hard
constraint, something that must stay or go, or an answer to an open question
(mark it `answered` and put the answer in `answer`). Do not invent these, and do
not use the brief as a place to store your own preferences.

`proposal decisions` returns every decided batch with the human's stated reason.
A rejected batch must not be re-proposed unchanged: either address the reason or
ask the user. Accepted batches explain what the resume already contains.

## Archive/restore and layout

```bash
"$SKILL_DIR/folio" resume get <resumeId>
"$SKILL_DIR/folio" resume archive <resumeId> --version <latestVersion>
"$SKILL_DIR/folio" resume restore <resumeId> --version <latestVersion>
"$SKILL_DIR/folio" layout diagnose <resumeId> --target-pages 1
"$SKILL_DIR/folio" layout diagnose <resumeId> --file ./next-document.json
"$SKILL_DIR/folio" layout diagnose <resumeId> --proposal <proposalId>
```

Archive/restore is only for explicit user requests; restore means archived, not
trash. Both increment saved version but do not rewrite the accepted body.

Diagnosis runs the actual renderer and returns `source` telling you what was
measured:

- `saved` (bare form) — the content the user currently sees.
- `document` (`--file`) — a dry run of the whole document you are about to
  propose. Pass the same `title` / `target` / `sections` / `style` / `photo`
  object you will put in the Proposal; nothing is written and no version moves.
  Use this **before** submitting so pagination and overflow never reach the user
  as a surprise.
- `proposal` (`--proposal`) — a batch already submitted, measured as if
  accepted. Use it to check your own batch, and to re-check after the user edits.

`onePage.fits=true` provides `recommendedStyle`; false provides `sectionUsage`,
`estimatedOverflowPx` and `suggestedReductionLines` for fact-preserving
reduction. Stay within returned style constraints; changes still require a
Proposal. `--proposal` and `--file` are mutually exclusive, and a proposal that
belongs to another resume is `NOT_FOUND`.

## Completion

Report connection/account, selected resume ID + latest baseline, actual operation,
Proposal ID/status, accepted-content comparison and human next step. Distinguish
archive version changes from body changes. State export/accept/delete/share were
not executed. `auth logout` only on explicit request; it forgets the local token,
not remote authorization or an environment-provided `FOLIO_TOKEN`.
