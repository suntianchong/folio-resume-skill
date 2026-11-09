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
"$SKILL_DIR/folio" resume get <resumeId>
"$SKILL_DIR/folio" history list <resumeId>
"$SKILL_DIR/folio" history changes <resumeId> --since <oldVersion>
"$SKILL_DIR/folio" proposal list <resumeId>
"$SKILL_DIR/folio" proposal get <proposalId>
```

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

Create is blank; copy preserves source saved content/style and `derivedFrom`.
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
  "changes": [{"op": "update_style", "patch": {"accentColor": "#526150"}}]
}
```

Use a private temporary file (not the repo), then:

```bash
"$SKILL_DIR/folio" action schema proposal.submit
"$SKILL_DIR/folio" action validate proposal.submit --file "$proposal_file"
"$SKILL_DIR/folio" proposal submit --file "$proposal_file"
"$SKILL_DIR/folio" proposal get <proposalId>
"$SKILL_DIR/folio" resume get <resumeId>
```

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

## Archive/restore and layout

```bash
"$SKILL_DIR/folio" resume get <resumeId>
"$SKILL_DIR/folio" resume archive <resumeId> --version <latestVersion>
"$SKILL_DIR/folio" resume restore <resumeId> --version <latestVersion>
"$SKILL_DIR/folio" layout diagnose <resumeId> --target-pages 1
```

Archive/restore is only for explicit user requests; restore means archived, not
trash. Both increment saved version but do not rewrite the accepted body.
Diagnosis uses actual renderer on **saved** content. `onePage.fits=true` provides
`recommendedStyle`; false provides `sectionUsage`, `estimatedOverflowPx` and
`suggestedReductionLines` for fact-preserving reduction. Stay within returned
style constraints; changes still require a Proposal. Pending proposed documents
are not measured by this command; user previews them in Folio.

## Completion

Report connection/account, selected resume ID + latest baseline, actual operation,
Proposal ID/status, accepted-content comparison and human next step. Distinguish
archive version changes from body changes. State export/accept/delete/share were
not executed. `auth logout` only on explicit request; it forgets the local token,
not remote authorization or an environment-provided `FOLIO_TOKEN`.
