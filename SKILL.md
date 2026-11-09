---
name: folio-resume-workbench
description: Use the user's Folio CLI to read saved resumes/history, create or copy independent drafts, diagnose layout, archive/restore on request, and submit whole-batch proposals for human review. Use for Folio resume tasks and pairing; never accept/reject, delete, export or share.
metadata:
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

# Folio Resume Workbench

Folio has no built-in AI. You are the user's external Agent; use business
actions, not SQLite, browser DOM/storage, source files or original attachments.

## Tool and connection

`SKILL_DIR` is the directory containing this file. Invoke `"$SKILL_DIR/folio"`;
caller cwd need not be the project. Node 24+ and a built Folio CLI are required.
When copied into an Agent's skill directory, set `FOLIO_PROJECT_ROOT` to the
absolute Folio project/client directory, or `FOLIO_CLI_BIN` to an already
installed CLI's absolute path. No automatic software installation or builds.
Installation and the shortest handoff message: [START-HERE.md](START-HERE.md).

1. Run `doctor`, then `auth status`; verify returned origin and account against
   the user's intended environment/email. An offline/old server is a maintainer
   stop, not permission to start services or modify payloads.
2. Only if not connected, use the single resumable pairing flow:
   `auth login --name "<client>" --email "<user-confirmed email>" --no-wait`.
3. Send the returned `verificationUri` to the user **as a clickable link**, and
   open it in a browser yourself when one is available (`open`/`xdg-open`/`start`).
   Say: **“请在 Folio 核对账号与权限并确认；不要发送 Token。确认后告诉我继续。”**
   If no browser can be opened, give the link and wait — never retry pairing in a
   loop or approve on the user's behalf. A signed-out user signs in first and is
   returned to the approval screen.
   Stop until the human approves. Then `auth complete --no-wait`, `auth status`.
   `awaiting_browser_approval` still means STOP, not success. Never approve via
   API/UI for a real user. `--no-wait` produces one JSON result per call.
4. On account/profile mismatch, stop. Use a separately authorized
   `FOLIO_CONFIG_DIR`; do not logout/replace old credentials without consent.
   Browser login and CLI identity are independent.

## Discover only what this task needs

- `help --json`: commands, flags, exits and environment (offline).
- `action catalog`: live account/scope-filtered permissions (connected).
- `action schema <actionName>` / `action validate <actionName> --file <json>`:
  offline input schema (including nested Proposal fields and refinements),
  not proof of factual truth, current version, permissions or actual page fit.
- For read/create/copy/archive/Proposal workflows, read
  [references/tasks.md](references/tasks.md).
- For complete rewriting or explicitly requested synthetic portrait, read
  [references/complete-resume.md](references/complete-resume.md).
  [COMPLETE-RESUME-TASK.md](COMPLETE-RESUME-TASK.md) remains a fictional task card.

## Invariants

- Use only the user-selected resume. Re-read latest saved `resume get` and
  numeric version immediately before proposing; never invent identity,
  experience, dates, skills, metrics or other facts.
- Existing content/style/order changes go through one whole-batch Proposal.
  Submission is not application. Never accept/reject, delete/trash, export,
  back up, share, publish or send.
- Create/copy and archive/restore require the user's request. Copies use the
  latest source version; no quota bypass.
- Writing/layout tasks use `layout diagnose <resumeId> --target-pages 1`;
  never guess page capacity or claim pending content was actually measured.
- `STALE_VERSION`: re-read resume + `history changes`, re-evaluate the whole
  batch, use fresh IDs/key and latest `baseVersion`; use
  `supersedesProposalId` for a pending batch being replaced. No old-payload retry.
- After submitting, re-read Proposal + resume. Report actual status, baseline,
  whether accepted content changed, and the user's next step (whole-batch review).
  If another actor saved meanwhile, report that conflict; do not overwrite.
