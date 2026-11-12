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
- For deriving a role-specific demo from an existing resume, cleaning up merged
  imported entries, 1-page fitting loops, or error recovery, read
  [references/playbook-and-pitfalls.md](references/playbook-and-pitfalls.md).
- Whenever drafting, diagnosing, rewriting, mining experience, or tailoring
  resume prose to a JD, read [references/writing-craft.md](references/writing-craft.md)
  (3-layer HR optimization: information hygiene, business-value translation
  under `no_new_facts`, and fact-only experience mining via `brief.questions`).
- When targeting a specific company type (大厂 / 国央企 / 银行金融 / 外企) or
  role track (PM/FDE, Eng, Data, Ops, Marketing, Sales, Design, Finance, Audit,
  Legal, HR, Consulting), read
  [references/industry-and-role-patterns.md](references/industry-and-role-patterns.md).

## Invariants

- Read in ascending cost. `resume list` returns summaries plus account `quota`
  (`used`, `limit: 10`, `remaining`, `active`, `archived`, even when archived
  resumes are omitted from `resumes[]`) — never body text or image data. Then
  `resume outline <id>` for structure (ids, headings, dates, sizes), then
  `resume get <id>` for prose (plus `lastChange` showing whether a human or
  Agent saved the latest version). The photo is omitted unless the user asks about
  the portrait, and only then use `resume get <id> --photo`; never paste base64
  or data URLs back into the conversation.
- Read `brief get <resumeId>`, `target get <resumeId>` (when `target.hasJd` or
  `target.role` is set in `resume list` / `resume outline`), and
  `proposal decisions <resumeId>` before you propose anything. Together they are
  the target JD, goal, constraints, what was already accepted or rejected, and why —
  recorded by earlier sessions so you do not have to interview the user again.
  Do not re-derive them, and do not re-propose a rejected batch unchanged.
- When the user pastes or describes a target role / company / JD in chat, persist
  it with `target update <resumeId> --role "..." --company "..." --jd-file <jd.txt>`
  (or `--jd "..."`). Like `brief`, `target` never enters the exported resume or
  PDF, **but `target update` advances `resume.version` (unless unchanged)** so an
  older browser tab cannot silently overwrite the saved JD. Therefore, **always run
  `target update` BEFORE `proposal submit`**, and use the `version` returned by
  `target update` as your `baseVersion`.
- When the user states a goal, an audience, a constraint, something that must
  stay or go, or answers one of the open questions, record it with
  `brief update`. The brief is context: it never enters the resume body or the
  PDF. Send only what changed and pass the current `expectedBriefVersion`.
  If `brief update` or `target update` fails for a missing `brief:write`
  capability, the connection was
  paired before this scope existed: say so, ask the user to re-pair, and carry on
  without recording rather than silently dropping what they told you.
- `proposal list` returns summaries (status, intent, operations). Read one
  proposal with `proposal get` only when you need its exact changes.
- Every batch you submit must state its own reasoning, because the reviewer sees
  the proposal without your chat: `notes.rationale` (one entry per changed item,
  `target` = `document` / `style` / `photo` / `target` / `order` /
  `section:<id>` / `entry:<id>`), `notes.tradeoffs` (what the user gives up),
  `notes.rejected` (options you considered and dropped, with the reason) and
  `notes.openQuestions`. The next Agent inherits these; do not rely on being the
  only Agent that will ever touch this resume.
- After submitting, run `proposal diff <proposalId>` and check it against what
  you intended: anything in `removed` disappears from the user's resume when
  they accept, and the diff is what the human will actually read. Fix the batch
  rather than explaining it in chat.
- Then run `proposal preview <proposalId>` for any batch where layout matters
  (a full rewrite, a cut for one page, added sections). It renders the exact PDF
  and returns `pages`, `onePageFits`, `layoutUnsafe`, a short-lived `url`, page
  images and `pdfUrl`. **Give the human the `url`** — do not describe the layout
  in words, and do not claim a page count you did not get from `pages`. If
  `layoutUnsafe` is true there is no PDF: something cannot be split across
  pages, so shorten that entry and resubmit instead of promising it will fit.
- Use only the user-selected resume. Re-read latest saved `resume get` and
  numeric version immediately before proposing; never invent identity,
  experience, dates, skills, metrics or other facts.
- Existing content/style/order changes go through one whole-batch Proposal.
  Submission is not application. Never accept/reject, delete/trash, export,
  back up, share, publish or send.
- Create/copy and archive/restore require the user's request. Check `quota`
  first (or pre-flight with `resume copy ... --dry-run` / `resume create ...
  --dry-run`, which validates version and quota without consuming a slot). When
  deriving or tailoring a new resume from an existing resume (e.g. a demo or
  role-specific cut from a base resume), ALWAYS use `resume copy <sourceId> --id
  <newId> --title "<title>" --version <latestSourceVersion>` rather than `resume
  create`: `copy` avoids a blank v1 intermediate state, records `derivedFrom`,
  inherits `brief`, and lets `proposal diff` show every removed, changed and
  kept entry against the source resume. Use `resume create` only when starting
  from scratch with no source resume in Folio. No quota bypass.
- Writing/layout tasks must be measured, not estimated. `layout diagnose
  <resumeId> --file <document.json>` dry-runs the document you are about to
  propose; `--proposal <proposalId>` measures a batch already submitted; the
  bare form measures saved content. The response's `source` (`saved` /
  `proposal` / `document`) states which one you measured — quote it accurately,
  and never claim a fit you did not measure.
- `STALE_VERSION`: re-read resume + `history changes`, re-evaluate the whole
  batch, use fresh IDs/key and latest `baseVersion`; use
  `supersedesProposalId` for a pending batch being replaced. No old-payload retry.
- After submitting, re-read Proposal + resume. Report actual status, baseline,
  whether accepted content changed, and the user's next step (whole-batch review).
  If another actor saved meanwhile, report that conflict; do not overwrite.

## Pitfall quick-check

| Scenario | Never do this | Do this instead |
| :--- | :--- | :--- |
| **Derive a demo/cut from an existing resume** | `resume create` (leaves a blank v1 draft and hides what was cut from source) | `resume copy <sourceId> --id <newId> --version <v>` (preserves baseline, `derivedFrom` and `brief`) |
| **Locate resumes or inspect structure** | `resume get --photo` across resumes (400KB+ Base64 blows up context) | `resume list` ➔ `resume outline <id>` ➔ `resume get <id>` (omit `--photo`) |
| **Imported resume has merged entries** | Edit prose inside a single merged entry or leave items in `"其他原文内容"` | Use `replace_document` to split distinct companies/projects into separate `entries[]` and explain in `notes.rationale` |
| **Fit onto 1 page** | Guess page fit from character count or describe layout in words | Dry-run `layout diagnose <id> --file <doc.json>` before submitting; run `proposal diff` + `proposal preview` after and share `url` |
| **Proposal rejected or session resumed** | Re-submit the same batch or re-interview the user from scratch | Read `brief get <id>` + `proposal decisions <id>` (`reason`); use `supersedesProposalId` when replacing a pending batch |
| **Save a pasted JD and submit a Proposal** | Embed JD inside a giant `replace_document` just to store it, or run `target update` *after* `proposal submit` (makes the proposal stale) | Run `target update <id> --role ... --company ... --jd-file ...` **first**, then submit Proposal at the returned `version` |
| **Missing metrics or vague prose (`大量/很多`)** | Invent numbers (`提升 35%`) or leave `[待补充]` placeholders inside resume body | Write factual Action + Mechanism prose, ask 3–5 concrete fact questions, and record them in `brief.questions` + `notes.openQuestions` |

