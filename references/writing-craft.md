---
event_date: "2026-11-12"
recorded_at: "2026-11-12 00:40:00"
domain: "interview"
type: "component"
client: "codex"
author: "agent"
agent_role: "cli_skill_auditor"
status: "active"
superseded_by: null
---

# Resume Writing Craft (简历内容打磨、价值翻译与经历挖掘方法论)

> Adapted from open-source HR advisory methodology (`industry-resume-toolkit`) and bound to Folio's strict `factBasis: "user_provided"` + `polishPolicy: "no_new_facts"` governance. Read alongside [industry-and-role-patterns.md](industry-and-role-patterns.md) when targeting a specific company type or role.

---

## 0. Fidelity Red Line × Folio Protocol (保真铁律与协议映射)

1. **Zero Fabricated Facts (`no_new_facts`)**:
   - Never invent companies, dates, roles, tools, team sizes, or numeric metrics (`提升 35%`, `处理 10w+ 数据`) that the user did not provide or confirm.
   - **Interview Stress-Test Rule**: Every phrase you write must survive 3 levels of follow-up questioning in a live interview. If a framing inflates "assisted with data export" into "architected the analytics system", reject it and stay conservative.
2. **Never Leave Inline Placeholders Inside Resume Prose**:
   - Do **not** write `[需要你确认：多少笔]` or `XX%` inside `entries[].body` — accepted Proposals render directly into the user's formal PDF.
   - Instead, when a bullet lacks metrics:
     1. Write a clean, factual **Action + Mechanism + Scope/Deliverable** sentence using only confirmed facts;
     2. Record the missing metric question in `folio brief update` (`questions[]` with `status: "open"`) **and** in `proposal submit` (`notes.openQuestions`);
     3. Ask the user 3–5 high-leverage factual questions in chat so the next Proposal batch can plug in real numbers.

---

## 1. Six-Point Target Diagnosis (`brief get` ➔ `brief update`)

Before rewriting prose, run `folio brief get <resumeId>`. If target context is missing, infer what you can from the existing resume + user prompt, ask only what is still unknown, and persist into `brief`:

1. **Career Stage**: Internship (`在校实习`) / New Grad (`校招应届`) / Junior (`社招 1-3 年`) / Mid-Senior (`社招 3-7 年 / 7+ 年`)
2. **Target Role**: Specific role title (e.g. `字节跳动 AI 产品经理`, `FDE`, `四大审计`, `招行总行管培`)
3. **Target Industry**: Internet/AI, Finance, FMCG, Consulting, Manufacturing, SaaS, Legal, etc.
4. **Enterprise Archetype** *(governs overall tone — see [industry-and-role-patterns.md](industry-and-role-patterns.md))*:
   - `大厂 (互联网/科技)` | `国央企` | `银行/金融机构` | `外企/跨国公司` | `私企/创业公司`
5. **Career Transition Mode**: Same track (`不转行`) / Role switch (`同行业换岗`) / Industry switch (`同岗换行业`) / Dual switch (`双跨`) / Gap restart (`空档期重启`) / Returnee (`海归回国`)
6. **Language & Template Fit**:
   - Chinese (`zh`): prefer `cn-classic` (`808.87px` single-page capacity, highest density for rich Chinese resumes), `rhyhorn` (`760px`, tech/FDE single-column), or `onyx` (`760px`, general internet single-column).
   - English (`en`): prefer `en-classic` (`695.3px`, Letter) or `en-engineering` (`684.71px`, dense technical/research serif).

---

## 2. The Three-Layer Optimization Framework (三层改法框架)

Calibrate effort across the three layers based on candidate material density:
- **Rich-experience resumes** (2+ solid internships/jobs): Light Layer 1 ➔ **Heavy Layer 2** (relevance cuts, value translation, crisp lead-ins) ➔ Targeted Layer 3 (top 1–2 entries only).
- **Thin-experience / early-career resumes** (≤1 internship or mostly campus/course work): **Heavy Layer 1** ➔ Medium Layer 2 ➔ **Heavy Layer 3** (mine course projects & campus execution into structured project entries).

### Layer 1 — Information Hygiene & Anti-AI-Slop (信息卫生与排雷)

- **Purge AI / Self-Praise Filler**: Delete empty adjectives and buzzwords (`赋能`、`抓手`、`打造闭环`、`多维度`、`无缝衔接`、`责任心强`、`沟通能力强`、`抗压能力强`、`获得领导一致好评`). Show concrete actions and outcomes instead.
- **Crisp Bold Lead-Ins**: When bullets use bold prefixes (`TextRun` with `bold: true`), keep them to **2–5 noun/keyword tokens** hitting role keywords, never self-congratulatory clauses:
  - Bad: `**高效激活用户参与度并精准定制宣发策略**：...`
  - Good: `**内容分发策略**：...` / `**私有化交付排障**：...` / `**漏斗转化分析**：...`
- **Ban Vague Quantifiers**: Eliminate `很多`、`大量`、`诸多`、`多个`、`丰富`. Either replace with user-confirmed numbers (`30+ 家客户`, `日均 2,000+ 条`) or describe the exact mechanism/scope (`覆盖华东区 KA 零售客户`, `贯穿需求评审、灰度验证与全量发布三阶段`) and log a metric question in `brief.questions`.
- **Contact & Header Hygiene**:
  - Never include QR codes; note WeChat inline with phone if needed (`13800000000（微信同号）`).
  - Omit redundant personal trivia (full street address, multiple emails) *except* when targeting `国央企` where complete demographic fields (`籍贯 / 政治面貌`) are expected.
- **Certificates, Courses & Skills Hygiene**:
  - **No "preparing/studying" certificates**: Delete `备考 CPA / 雅思`. If partial subjects are passed, write exact facts: `CPA（已通过会计、审计 2 科）`.
  - **All-or-nothing course grades**: Do not list grades on 3 cherry-picked courses while hiding others (signals weak grades elsewhere); either list core relevant courses cleanly or attach overall GPA/rank if strong.
  - **Separate courses from tools**: Never list `Excel` or `Python` inside `主修课程`; place them in `skills` (`工具栈 / 计算机能力`).
  - **Consolidate commodity Office tools**: Unless targeting `国央企` (which prefers explicit `熟练使用 Word、Excel、PPT`), write `Office（熟练使用 Excel Vlookup / 数据透视表）` instead of listing Word/Excel/PPT separately.
  - **Merge honors under Education**: Avoid a standalone 1-line `获奖荣誉` section when vertical space is tight; attach scholarships/honors concisely under `education` to save `sectionGap` + heading height (~26–32px).

### Layer 2 — Business Value Translation (业务价值翻译)

Turn passive job descriptions ("what tasks were assigned to me") into high-signal impact bullets ("how my action solved a problem or moved a metric").

- **Core Formula**: **[Strong Action Verb] + [Target Scenario / Problem] + [Concrete Method / Tool (with parenthetical specifics)] + [Confirmed Result or Verifiable Deliverable]**
- **Parenthetical Specificity**: Whenever naming a framework or model, unpack its actual dimensions in parentheses:
  - Weak: `采用用户分层模型优化推送策略`
  - Strong: `基于用户分层模型（活跃频次、近 30 日留存、付费转化阶段）重构触达策略`
- **Before / After Examples (Strict `no_new_facts` Compliance)**:
  - *Raw user text*: `负责整理客诉表格，每周用 Excel 汇总发给产品和研发推动修复。`
  - *Bad rewrite (hallucinated metrics)*: `主导搭建客诉自动化治理平台，将工单响应效率提升 45%，客诉率下降 28%。` *(FORBIDDEN — invented platform & numbers)*
  - *Good rewrite (when numbers are NOT yet provided)*: `**客诉归因与闭环跟进**：使用 Excel 数据透视表对周度客诉工单做问题聚类与优先级标注，建立跨产品、研发的周度同步跟进表，推动高频缺陷按周闭环。` *(Plus add to `brief.questions`: "每周大约处理多少条客诉？高频问题修复率或响应周期是否有统计？")*
  - *Good rewrite (after user confirms `周均 150+ 条，Top 5 问题两周内清零`)*: `**客诉归因与闭环跟进**：使用 Excel 数据透视表按模块聚类周均 150+ 条客诉工单，联动产品与研发建立周度排期跟进机制，推动当期 Top 5 高频缺陷在 2 周内清零。`
- **Conservative Title Normalization**:
  - Allowed: Clarifying opaque internal titles to standard market equivalents matching actual duties (`版权运营` ➔ `内容运营`, `需求分析专员` ➔ `产品经理（需求分析方向）`, `HRBP 助理` ➔ `HRBP（助理岗）`). Explain the normalization in `notes.rationale`.
  - Forbidden: Escalating seniority or scope (`实习生` ➔ `项目负责人`, `开发工程师` ➔ `技术总监`).

### Layer 3 — Fact-Only Experience Mining (经历挖掘与四大转折期打法)

Trigger Layer 3 when key entries are 1-line stubs, contain 3+ vague words (`大量/很多`), or lack deliverables.
- **Golden Rule**: **Ask ONLY for concrete facts, NEVER motives or feelings.** Never ask *"你为什么想转行/回国？"* or *"你觉得自己最大的优势是什么？"* (interview talk). Ask *"做过什么模块 / 用了什么工具 / 处理多大样本 / 交付了什么文档或代码？"* (resume facts).
- **Batch Limit**: Ask **at most 3–5 targeted questions per turn**, store them in `brief.questions`, and let the user say "不知道/没统计" without pressure.

#### Career-Transition Mining Playbooks:
1. **Fresh Grad / Zero Formal Internship (`应届 0 经验`)**:
   - Mine **complex course projects / capstones** (treat a rigorous course project like an internship entry: team size, personal module, method/code/dataset scale, final deliverable/grade).
   - Mine **campus orgs / competitions / self-media / part-time gigs** as structured projects (`背景 + 协调人数/预算 + 落地动作 + 覆盖人数/转化结果`).
2. **Career Switcher (`转行求职`)**:
   - Mine **transferable execution facts** from prior work (data scale handled, cross-team coordination, client delivery, process automation).
   - Explicitly surface a dedicated **`新方向相关项目 / 准备`** signal (relevant side projects, completed certifications, bootcamps, internal cross-functional projects, technical blog/GitHub). Never write an emotional "why I want to switch" essay in the summary; use factual positioning: `"X 年 [原领域] 经验，沉淀 [可迁移能力事实]，已完成 [新方向项目/证书事实]，聚焦 [目标岗位]。"`
3. **Career Gap Restart (`Gap 期重启`)**:
   - Keep timeline clean and factual; never write defensive apologies.
   - Mine **gap-period concrete outputs** (certifications passed, structured coursework, freelance/consulting deliveries, open-source/personal builds, volunteer operations) so active capability maintenance is visible.
4. **Overseas Returnee (`海归回国`)**:
   - Pair unfamiliar overseas university names with factual anchors (`XX University（QS Top 50，金融工程硕士）`) when targeting domestic HR.
   - Add concise 1-phrase benchmark context for lesser-known overseas employers (`XX Corp（北美头部 SaaS 服务商）`) if truthful.
   - Prepare bilingual framing (`zh` + `en` independent drafts via `resume create` / `resume copy` when user requests both).

---

## 3. JD Matching & Tailoring Protocol (`JD 匹配` ➔ Folio Proposal)

When the user provides a target JD:
1. **Decompose the JD into 4 buckets**:
   - Hard gates (degree, years, mandatory licenses/languages)
   - High-frequency domain verbs & nouns (repeated ≥2–3 times)
   - Bonus signals (`优先 / 加分项`)
   - Enterprise archetype & team culture signals
2. **Audit the user's Folio resume against the JD**:
   - **Covered (`✅ 已覆盖`)** ➔ Add to `brief.mustKeep`; move to top of relevant section or highlight in summary.
   - **Under-expressed (`⚠️ 表达不到位，可挖掘重述`)** ➔ Rewrite existing real experience using the JD's exact domain terminology (no new facts), or ask 1–2 factual questions in `brief.questions`.
   - **True Gap (`❌ 真实缺失`)** ➔ Never fabricate; disclose honestly to the user and note trade-offs in `notes.tradeoffs` / `notes.openQuestions`.
   - **Low-relevance (`✂️ 弱相关/占版面`)** ➔ Compress or cut when fitting onto 1 page (`resume copy` + `layout diagnose --file`), recording each cut in `notes.rationale` (`{"target": "entry:<id>", "reason": "..."}`) so `proposal diff` explains every editorial decision to the human reviewer.
