---
event_date: "2026-11-12"
recorded_at: "2026-11-12 00:40:30"
domain: "interview"
type: "component"
client: "codex"
author: "agent"
agent_role: "cli_skill_auditor"
status: "active"
superseded_by: null
---

# Industry & Role Patterns (四大企业性质与十二类核心岗位写作范式)

> Progressive-disclosure domain rulebook distilled from `industry-resume-toolkit` and adapted to Folio's templates, layout constraints, and `no_new_facts` boundary. **Specific enterprise + role patterns take precedence over generic writing advice.**

---

## Part 1 — Enterprise Archetype Matrix (企业性质决定整体语气与取舍)

Always identify the target enterprise archetype first — a bullet style that wins at an internet giant can hurt a candidate at a central SOE, and vice versa.

| Dimension | 大厂 / 互联网科技 (Big Tech & AI) | 国央企 (Central & Local SOEs) | 银行 / 金融机构 (Banking & Finance) | 外企 / 跨国企业 (MNCs & English CV) |
| :--- | :--- | :--- | :--- | :--- |
| **Core Vibe** | 量化、JD 命中、敏捷闭环、数据驱动、用户/业务视角 | 稳重、规范、信息完整、组织协同、长期主义 | 严谨、合规、风控意识、口径精准、证书硬通货 | Concise, STAR, Strong Action Verbs, Cross-functional |
| **Recommended Folio `templateId`** | `cn-classic` (dense) / `rhyhorn` (tech/FDE) / `onyx` | `cn-classic` (clean serif, structured identity grid) | `cn-classic` (zh) / `en-classic` (foreign banks) | `en-classic` (general) / `en-engineering` (tech/quant) |
| **Length & Cutting Strategy** | Ruthless 1-page curation: cut unrelated campus fluff; keep high-impact projects | **Completeness > minimalism**: full chronological education & internships; don't leave unexplained gaps | Strict 1 page (campus/junior); dense, zero-typo precision | Strict 1 page (<3–5 yrs), max 2 pages (senior); 6-second scan clarity |
| **Verb Tone** | Ownership verbs: `主导 / 推动 / 搭建 / 验证 / 落地` (within truth boundary) | Modest, collective verbs: `参与 / 协助推进 / 在团队中承担 / 优化改进` (avoid `颠覆/重塑`) | Precise professional verbs: `独立完成抽凭 / 搭建估值模型 / 核查合规风险` | Past-tense strong verbs: `Led, Built, Designed, Reduced, Accelerated, Automated` (avoid `Helped / Responsible for`) |
| **High-Value Signals** | Business metrics (`DAU/GMV/留存/P99/交付周期`), A/B testing, 0→1 or scale | `中共党员/预备党员` (if true), student union / Youth League leadership, `国家奖学金/挑战杯`, major strategic projects | `CFA/CPA/FRM/法考/证券从业`, exact units (`bps`, `pp`, `YoY`, `AUM`), core finance courses + grades | Global/cross-regional collaboration, bilingual fluency (`IELTS 7.5+`), standardized international units (`$2.5M`, `YoY`) |
| **Instant Red Flags (地雷)** | Empty self-praise (`沟通能力强`), vague `参与/了解` everywhere, unquantified walls of text | Over-claiming solo heroism (`凭一己之力颠覆`), flashy internet slang (`抓手/赋能`), missing basic education details | Vague numbers (`约 1 亿`), misused financial terms (`市盈率 vs 市净率`), inflating lobby/teller errands into IBD lead | Passive voice, Chinglish, paragraphs instead of STAR bullets, >2 pages |

### Sub-ecosystem Nuances (Tech / Internet):
- **ByteDance / AI Startups**: Data-driven experimentation (`A/B 测试`、`指标拆解`), rapid iteration, high ownership, end-to-end delivery.
- **Alibaba / E-commerce (Meituan, PDD, JD)**: Commercial sense (supply × demand), pragmatic execution (`打硬仗`、`ROI / 成本效率`、`落地转化`).
- **Tencent / Content Ecosystems (Xiaohongshu, Bilibili)**: Product intuition, user empathy, creator/community ecosystem understanding.

---

## Part 2 — Role-Specific Patterns (十二类岗位核心信号与防坑清单)

Combine Part 1's enterprise tone with the target role's specific rules below:

### 1. 产品经理 / AI PM / FDE (Forward Deployed Engineer)
- **Structure**: `[Target User / Enterprise Pain Point in Scenario] ➔ [Product / Solution Architecture & Trade-off] ➔ [Cross-team / Client Delivery] ➔ [Adoption, Retention, Efficiency or Revenue Metric]`.
- **FDE / ToB AI Specifics**: Highlight messy real-world deployment facts — data pipeline adaptation, private/on-prem delivery constraints, evaluation/guardrail loops, POC-to-production conversion, and direct C-level/business stakeholder alignment.
- **Tools**: Layer cleanly (`原型与协作: Figma, Notion/飞书` | `数据与分析: SQL, Python, AB 实验` | `AI 工程栈: Prompt/Eval/Agent 工作流` — list only genuinely used tools).
- **Red Flags**: Feature-list syndrome ("上线了 A、B、C 三个功能" with zero user pain point or outcome).

### 2. 开发 / 算法 / AI 工程师 (Software, Infra, Data & ML Engineering)
- **Structure**: `[Business/System Context & Bottleneck] ➔ [Technical Design / Architecture / Algorithm Choice] ➔ [Engineering Metrics]`.
- **Engineering Metrics**: Latency (`P99 耗时`), throughput (`QPS / TPS`), reliability (`可用性 / 故障率 / MTTR`), data volume, build/deploy efficiency, or model evaluation + online business lift.
- **Skill Proficiency Honesty**: Tier skills (`熟练掌握` vs `熟悉`) — never mark a technology `精通` unless prepared for deep internals questioning. Include GitHub / open-source PRs / papers / competition ranks if real.
- **Red Flags**: Listing team-wide microservices architecture without isolating **your personal module and technical contribution**.

### 3. 数据分析 / 商业分析 (Data & Business Analytics)
- **Structure**: **Analysis Process + Business Conclusion/Adoption** (never stop at "built a dashboard" — state what decision the dashboard or model changed).
- **Statistical Rigor**: Distinguish `A/B 测试` (sample size, significance, lift) from simple before/after comparison; distinguish correlation from causation; anchor conclusions in business KPIs (`GMV / 留存 / 坏账率`) rather than isolated ML metrics alone.
- **Scale Signal**: State row/table/department scale (`清洗 X 万条行为日志`、`覆盖 3 个业务部门的周度经营看板`) when confirmed by user.

### 4. 运营 (内容 / 用户 / 活动 / 社区 / 电商运营)
- **Crisp Subheadings**: Lead bullets with exact sub-track keywords (`**选题规划**`、`**用户分层**`、`**活动复盘**`、`**选品测款**`、`**转化漏斗**`).
- **Method + Parenthesis**: Always explain segmentation or growth models in parentheses (`基于活跃度、消费频次与停留时长的用户分层`).
- **Metrics**: Reach/exposure, CTR, engagement rate, retention, ROI, GMV, community conversion.

### 5. 市场营销 / 品牌 / 投放 / 公关 (Marketing & Growth)
- **Campaign Arc**: Frame work across insight (`目标人群画像`) ➔ channel/creative strategy (`跨平台矩阵 / KOL 协同`) ➔ efficiency/brand result (`CPM/CPC/ROAS`、曝光量、UGC 量、线索转化).
- **Early-Career Boosters**: Campus ambassador metrics, personal social accounts with real traction, commercial case competitions (`Brandstorm` etc.).

### 6. 销售 / BD / 客户成功 (Sales, BD & CSM)
- **Lead with Hard Quota Numbers**: Revenue delivered, quota attainment (`完成率 X%`), team rank (`Top X%`), new logos signed, pipeline conversion (`Lead ➔ Won`), or SaaS `ARR / NRR / 续费率`.
- **Pair Numbers with Method**: Specify customer tier (`KA / SMB / 政企`), CRM discipline (`Salesforce / 纷享销客`), and account expansion playbooks.

### 7. 设计 (UI / UX / 产品设计 / 视觉品牌)
- **Portfolio First**: Ensure the portfolio URL (`website`) is prominently placed in `basic` (`entries[0].website`) so Folio renders a clean link in the header.
- **Case Study Structure**: `User/Brand Problem ➔ Research/Design System/Interaction Solution ➔ Usability or Business Outcome` (task completion time, conversion lift, component reuse count).

### 8. 财务 / FP&A / 财务 BP (Corporate Finance & Accounting)
- **Hard Credentials**: Highlight `CPA（已通过科目）`、`ACCA`、`CMA`、accounting titles, and ERP systems (`SAP / Oracle / 用友 / 金蝶`).
- **Accounting-Grade Precision**: Specify closing cycle reduction (`月结从 X 天缩至 Y 天`), entity scope (`覆盖 N 家子公司合并报表`), accounting standards applied (`CAS / IFRS / US GAAP`), and exact currency/unit basis.

### 9. 审计 (External & Internal Audit)
- **Procedural Clarity**: Separate multi-step audit procedures cleanly with semicolons (`抽凭测试；底稿编制；往来及银行询证函发函与回函差异核对`).
- **Problem Resolution Signal**: Highlight handling of exceptions (e.g. confirmation discrepancies, cut-off testing adjustments) and data-audit tools (`Excel 高级函数 / Python / Power BI / SAP`).

### 10. 法务 / 律师 (Legal & Compliance)
- **Credentials & Education**: Surface `法律职业资格证（A 证）`, bar admissions (`NY Bar` etc.), core law courses, moot court, and legal English capability prominently.
- **Matter-Level Specificity**: State matter type (`跨境并购 / IPO / 诉讼仲裁 / 数据合规审查`), transaction/dispute scale (anonymized per confidentiality!), concrete legal actions (`尽职调查 / 起草审阅中英文协议 / 检索备忘录`), and deliverables.

### 11. 人力资源 (HRBP / 招聘 / 培训 / 薪酬绩效)
- **HR Funnel & Business Metrics**: Recruitment (`offer 数、平均招聘周期、渠道转化漏斗、到岗率`), Training (`覆盖人次、训后评估`), C&B (`调薪测算覆盖规模`), HRBP (`支持团队规模、组织诊断与人才盘点`).
- **Compliance & Tooling**: Labor law compliance (`劳动合同法` dispute prevention/handling) and HRIS/ATS familiarity.

### 12. 战略 / 管理咨询 (Strategy & Consulting)
- **Pyramid & Case Structure**: Every project entry reads like a mini case: `[Client Industry & Strategic Question] ➔ [Workstream Owned: Market Sizing / Expert Interviews / Financial Modeling / Competitor Benchmarking] ➔ [Quantified Strategic Recommendation / Deliverable]`.
- **Signals**: Academic distinction, leadership roles in high-selectivity organizations, crisp 1-page formatting, zero fluff.
