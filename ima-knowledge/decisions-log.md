# 决策日志（Decisions Log）

> 关键决策与待办。副机做任何决策前先查此文件，避免重复决策 / 冲突。

## 已定决策
- **2026-07-13｜Waffo KYB 批准**：店铺「已审核」，解锁上架与真实收款。唯一阻塞解除。
- **真实身份 = AI 编程驱动的欧美微 SaaS 工厂**：推翻早期 OPC 建盘期「科研图表」利基（与创始人真实方向冲突）。
- **定价档位**：B2B $19–$99 为主；R15 房产 Agent 用 $15/$19；消费端克制。
- **R11 三款赢家首发**：aiactradar($59) / agentredteam($79) / privacyscan($29)。2026-08-02 EU AI Act 高风险义务硬 deadline 为「为什么是现在」主轴。
- **全量真实 LLM 注入**（2026-07-13）：修复早期 36 款 `tool.ts` 无 key、线上吐 mock 却标榜 "AI-powered" 的退款/信誉隐患；注入 NVIDIA 三变量 + 重部署，REST 实测 44/44 全部 `OPENAI_API_KEY` 存在。
- **过度承诺修正**：删 2 处虚构 features（agentledger "Finance export"、sopforge "Export to docs"），外科手术式。Team/Studio 定价档**保留**（合法变现）。
- **18 个孤儿目录**（ai-changelog/invoice-gen 等，0 features、无 systemPrompt）为早期实验残留，不在在售清单，不构成隐患，**建议后续删目录**。
- **获客不买量**：Build in Public 优先。

## 待办 / 阻塞（需人工）
- **生产态真实收款**：当前 Waffo checkout 为**测试态**（`pancake.waffo.ai`）。KYB 已过后要让链接真实扣款，需把店铺切到生产态并生成生产态 checkout 链接。涉及真实资金，建议用户在 Waffo Dashboard 确认生产态切换方式后，再批量把文案里的测试链接替换为生产链接。
- **GA4 Measurement ID 缺失**：gtag.js 已接线到 R11 三款赢家应用（placeholder `G-REPLACE_ME`），但无真实 `G-XXXXXXX`，分析无数据。需用户提供。
- **IMA 连接器未连接**：本会话 IMA MCP disconnected，无法直推知识库。已整理 `IMA知识库/` 目录待导入（见 INDEX.md）。

## 进行中
- Waffo 补列 R12/R13/R14/R15（32 款，测试态）→ 生成 checkout 链接 → 更新注册表。
- gtag.js / GA4 dataLayer 事件接线（R11 三款赢家）。
- R14/R15 部署已完成（VERCEL REST 实测真实 LLM 生效）。
