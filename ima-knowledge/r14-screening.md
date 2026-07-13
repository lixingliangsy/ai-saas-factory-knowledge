# R14 市场筛选与选型笔记

生成日期: 2026-07-13
执行人: r14-dev-round (autonomous)
参考技能: `last30days`（沙箱内为交互式社交监听引擎，需向导式 setup + AskUserQuestion + 外部 API，无法在无人值守模式下运行；按任务授权回退至 WebSearch 做趋势/搜索量信号评估）

## 一、市场信号来源（WebSearch 回退）

| 信号 | 来源 | 要点 |
|------|------|------|
| HR/招聘 SaaS 是 2026 增速最快品类 | BigIdeasDB 营收情报（2,463 家初创） | Recruiting & HR 品类 **+101% 增长**，仅 74 家初创；Sales 工具 $6,091 平均 MRR、仅 52 家；Content Creation $15,921 平均 MRR |
| "AI for recruiters" 被列为低竞争高需求微 SaaS 模式 | tldl.io / ybuild.ai / questera.ai 2026 创意榜 | 反复出现：招聘信息撰写、合同审查、面试官、候选人筛选、Offer 信、入职计划、招聘外联 |
| AI 代理 / vibe coding / 招聘是 Product Hunt 2026 强信号 | BigIdeasDB SaaS Market Trends 2026 | "Product Hunt signals pointing toward vibe coding and AI agents" |
| 搜索需求激增（2026 Q2） | MicroNicheBrowser May 2026 surges | "career change at 40" 368k 月搜索；职业转型/招聘相关长尾词高增长 |

**结论：Recruiting & Hiring（HR-tech）品类具备"近 30 天强增长 + 低竞争 + 文本原生（适配本厂 LLM 文本生成骨架）"三重条件，且与现有 60 款产品零重叠。**

## 二、重叠检查（vs 现有 R1–R13）

现有产品覆盖：电商运营(R13: prodcopy/variatcopy/reviewminer/sizeguide/returncopy/upsellideas/storeseo/bundler)、开发者工具、内容/营销(caption-gen/headline-gen/briefkit/seo-*)、邮件(coldmail-ai/email-*)、健身健康、法律合规(policyforge/clauseguard/privacyscan)、AI 治理(agent*/aiactradar/promptshield)、简历(resumeforge，求职者侧) 等。

本簇为**雇主/HR/招聘官侧**工具，与 resumeforge（求职者简历润色）买家不同、slug 不同，属低重叠。8 个候选 slug 已逐一核对 `waffo_product_registry.json` 与 `12_Micro_SaaS出海/` 目录，均唯一（registry_matches=0，无目录）。

## 三、选定簇：Recruiting & Hiring（HR-tech）8 款

同簇 8 款共享买家（招聘官/HR/创始人），可互相导流（写 JD → 出面试题 → 筛候选人 → 发 Offer/拒信 → 做入职计划 → 做薪酬理由 → 写挖角外联）。

| # | slug | 品类 | 一句话价值 | 目标受众 | 定价（两档，禁 Team/Studio/Enterprise） |
|---|------|------|-----------|---------|----------------------------------------|
| 1 | jobdesc-ai | 招聘文案 | 从角色+级别+硬性要求生成结构化、包容性岗位 JD | HR / 招聘官 / 创始人 | Free $0 / Pro $19 |
| 2 | interviewq | 面试提效 | 按岗位生成带评分量表的面试题（行为+技术） | HR / 用人经理 | Free $0 / Pro $19 |
| 3 | candidatescore | 候选评估 | 粘贴简历+JD，输出结构化匹配度评分卡（优势/差距/证据） | 招聘官 / 用人经理 | Free $0 / Pro $19 |
| 4 | offerletter | Offer 文书 | 按岗位/薪酬/入职日生成规范 Offer 信（含辖区提示） | HR / 创始人 | Free $0 / Pro $19 |
| 5 | rejectmail | 候选沟通 | 分阶段的礼貌拒信（初筛/终面），多语气 | HR / 招聘官 | Free $0 / Pro $15 |
| 6 | onboardplan | 入职管理 | 按岗位生成 30/60/90 天结构化入职计划 | HR / 管理者 | Free $0 / Pro $15 |
| 7 | compat | 薪酬理由 | 从角色/地区/资历生成可辩护的薪酬理由与结构叙述（LLM 推理，非真实市场数据） | HR / 创始人 | Free $0 / Pro $15 |

> ✏️ 修正：上表第 7 行 slug 笔误为 `compat`，真实部署 slug 为 **`compreason`**（VERCEL REST API 实测确认：8/8 项目均 ENV_OK）。
| 8 | recruiterly | 人才外联 | 从候选人资料+岗位生成个性化被动候选人挖角外联（LinkedIn/邮件） | 招聘官 /  sourcer | Free $0 / Pro $19 |

## 四、硬规则遵守确认（历史 bug 防护）

- 每款仅 2 档定价（Free + Pro），无 Team/Studio/Enterprise 档。
- features 数组只写模板真实实现的能力（LLM 文本渲染到页面），不声称 export / version history / API access / team workspace / priority support / collaboration。
- pricing desc 不写 "export"，改用 "save history"。
- slug 全唯一；全英文 UI，生成文件零 CJK。
- 每款有真实可用的 systemPrompt，保证 /api/tool.ts 产出有用结果。
