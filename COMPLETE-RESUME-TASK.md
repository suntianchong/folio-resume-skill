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

# 完整简历润色＋合成头像任务

仅在用户主动请求此完整任务时加载，不是每次连接必读。
执行细节见 [references/complete-resume.md](references/complete-resume.md)；
复制安装与路径变量见 [START-HERE.md](START-HERE.md)。

把下面整段发给已连接 Folio 的外部 Agent。所有资料均明确为虚构测试数据。
将 `<FOLIO_PROJECT_ROOT>` 替换为本机项目或解压后的客户端目录；也可使用网页
“账户 → 外部 Agent 配置”生成的当前指令。

```text
继续使用已经连接的 Folio Resume Workbench Skill：
<FOLIO_PROJECT_ROOT>/apps/folio-cli/skill/SKILL.md

本次任务是通过你这个外部 Agent 提交一份可阅读完整简历的待审方案；
Folio本身没有生成能力，实际预览/接受/导出由我操作。

目标简历 ID：folio_agent_demo_01

以下所有资料均由我明确提供，属于虚构测试数据，不对应真人或真实公司。你可以
润色表达、压缩重复、调整结构和要点顺序，但不得新增任何公司、职位、日期、
指标、奖项、技能、项目或联系方式；不得把定性描述改成未经提供的量化成果。

姓名：林知夏（虚构）
目标职位：资深产品设计师
所在地：上海
邮箱：lin.zhixia@example.invalid
作品集：https://folio-demo.example

个人简介：
7年数字产品设计经验，专注复杂工作台、企业服务和跨端体验。
擅长用户研究、交互架构、原型设计和设计系统落地。

工作经历一：
公司：澄川科技（虚构）
职位：资深产品设计师
时间：2023.04 – 至今
内容：
- 负责企业协作工作台的端到端体验设计。
- 联合三个虚构业务团队建立设计系统和评审机制。
- 通过虚构可用性测试优化批量操作和权限提示。

工作经历二：
公司：远岚网络（虚构）
职位：产品设计师
时间：2020.07 – 2023.03
内容：
- 设计数据配置、数据探索和异常定位流程。
- 将筛选、图表和导出规范整理为可复用组件。
- 参与18场明确标为虚构的用户访谈记录整理。

项目经历：
项目：移动端审批重构（虚构）
角色：研究与交互设计
时间：2022
内容：
- 将多级表单改为按风险渐进呈现。
- 设计离线草稿和提交失败恢复路径。
- 在虚构测试中验证首次操作流程。

教育经历：
学校：东海大学（虚构）
专业：工业设计
学历：学士
时间：2016.09 – 2020.06

技能：
用户研究、交互架构、原型设计、设计系统、可用性测试、Figma、
基础 HTML/CSS、中文、英文工作沟通。

头像要求：
- 使用你自己的外部生图能力生成一张“合成虚构成年职业人物”头像；
- 中性纯色背景、头肩构图、职业着装、自然光、无文字、无 Logo、无水印；
- 不使用真人、网络图片、参考人脸或身份数据；
- 输出 PNG/JPEG/WebP；
- 在 Folio photo 中标记 source=external_agent_synthetic、fictional=true；
- style.photoMode=uploaded；
- 如果你无法调用生图能力，必须停止并告诉我，不得找网络图片或静默省略头像。

执行要求：
1. 先读取 folio_agent_demo_01 的最新版本和历史；
   并用 layout diagnose 对当前已保存内容诊断排版；不得盲猜容量，
   不得把该结果冒称为待审新正文的实测页数；
2. 根据以上资料润色并构建完整结构化简历；
3. 使用 Skill 的 replace_document，正文和头像作为同一批 Proposal；
4. factBasis=user_provided，polishPolicy=no_new_facts；
5. Proposal ID：proposal_folio_complete_resume_v1；
6. 幂等键：folio-complete-resume-v1；
7. 不能直接应用、接受、导出、删除、备份、分享或外发；
8. 提交后回读 Proposal 与简历，确认 Proposal=pending，当前已接受简历未变化；
9. 汇报章节、记录、头像来源、你做过的润色，以及所有未改变的事实；
10. 等我在 Folio 中看完整预览并整批接受或拒绝。
```
