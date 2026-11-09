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

# Complete rewriting; optional externally generated synthetic portrait

Only use `replace_document` when a complete draft/rewrite is explicitly
requested. Read latest saved resume, history and `layout diagnose` first.
Keep it the sole change; use `factBasis=user_provided` and
`polishPolicy=no_new_facts`. Every factual phrase must trace to user material.
Improve expression, not companies/roles/dates/metrics/skills/identity. If material
is insufficient, ask or propose structure only. Do not generate a portrait
unless requested.

```json
{
  "proposalId": "complete_example_v1",
  "idempotencyKey": "complete-example-v1",
  "resumeId": "user_selected_id",
  "baseVersion": 1,
  "intent": "根据用户提供事实组织完整简历，不新增事实",
  "changes": [{
    "op": "replace_document",
    "factBasis": "user_provided",
    "polishPolicy": "no_new_facts",
    "document": {
      "title": "用户指定标题",
      "language": "zh",
      "sections": [{
        "id": "basic", "kind": "basic", "title": "基本信息", "hidden": false,
        "entries": [{"id": "basic_entry", "heading": "", "subtitle": "",
          "date": "", "body": []}]
      }],
      "style": {
        "templateId": "cn-classic", "accentColor": "#345268",
        "fontSize": 10.5, "lineHeight": 1.45, "sectionGap": 12,
        "pageMargin": 30, "photoMode": "none", "photoShape": "square",
        "photoScale": 100, "photoX": 0, "photoY": 0, "pagePolicy": "auto"
      },
      "target": {"role": "", "company": "", "source": "", "jd": ""}
    }
  }]
}
```

This is a structural example, not a page-fit recommendation. Prefer the latest
saved controlled style plus diagnosis; use `action schema proposal.submit` for
all nested field schemas and its `additionalRules` refinements.
Section kinds: basic/education/experience/projects/skills/custom.
Each section needs at least one entry. Preserve stable IDs where appropriate.
Text blocks: `{id,type:"paragraph",runs:[{text,bold?}]}` or
`{id,type:"bullet"|"ordered",items:[[{text,bold?}]]}`.

If user requests a portrait, use the **external Agent's** image tool only:
unmistakably synthetic fictional adult, head/shoulders, plain background,
professional clothing, no text/logo/watermark. No real photo, network face or
identity reference. If unavailable, stop rather than silently substitute.

Add document `photo` with `mimeType` PNG/JPEG/WebP, `dataBase64` (no data URL
prefix, at most 8,000,000 characters), `fileName`, ISO `updatedAt`,
`source:"external_agent_synthetic"`, `fictional:true`; set `style.photoMode` to
`uploaded`. Whole document + portrait is a single pending batch.

Run local input validation then submit, read Proposal and resume back, and
compare saved version/content. Report facts retained, actual status and portrait
provenance. Diagnosis measures saved content only; never report the pending
rewrite's page count as measured. User inspects full preview and accepts/rejects;
you do not export or apply it. Fictional example task:
[COMPLETE-RESUME-TASK.md](../COMPLETE-RESUME-TASK.md).
