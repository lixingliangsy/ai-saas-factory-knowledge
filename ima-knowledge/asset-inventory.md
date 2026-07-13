# 资产登记表（Asset Inventory）

> 维护：资产沉淀师 沉墨（opc-asset-strategist）｜ 主理人 易牧 登记于 2026-07-12
> 用途：一人公司出海项目的可复用内容/代码资产总账。R12+ 直接复用，避免重复造轮子。

---

## 📦 内容资产（对外发布）

| # | 资产 | 路径 | 类型 | 状态 | 复用方式 |
|---|---|---|---|---|---|
| 1 | R11 Week-1 发布文案包（21 篇成稿） | `opc-doc/outputs/08-asset/r11-week1-launch-copy.md` | 英文发布文案 | READY（17 处 `[LIVE_SOON]` 已替换为真实 Waffo checkout 链接，带渠道 UTM） | KYB 后可直接点火（链接现指向 pancake.waffo.ai 测试态 checkout） |
| 2 | Week-1 GA4 埋点 + 测量框架 | `opc-doc/outputs/07-conversion/r11-week1-content-ga4.md` | 数据分析方案 | READY（待工程接 gtag.js） | 工程按 §B.5 接线 |
| 3 | 开发过程复盘 + 小红书种草文案 | `opc-doc/outputs/08-asset/r11-journey-summary-xhs.md` | 中文种草/复盘 | READY（合规 2026 规范） | 发布时勾选 AI 辅助创作 |
| 4 | R14 HR-tech 簇 8 款（已部署+真实 LLM） | `12_Micro_SaaS出海/<slug>/`（jobdesc-ai / interviewq / candidatescore / offerletter / rejectmail / onboardplan / compreason / recruiterly）。备注：r14-screening.md 行 37 笔误写作 `compat`，真实部署 slug 为 `compreason`（VERCEL REST 实测确认，8/8 全部 ENV_OK）。 | 微 SaaS 产品（英语） | DEPLOYED（8/8 真实 NVIDIA NIM 注入，VERCEL REST 实测确认） | 待 KYB 后上架 Waffo（补列进行中） |
| 5 | IMA 共享知识库（本机+副机协同） | `IMA知识库/`（INDEX / products-catalog / skills-inventory / conventions / decisions-log） | 跨机知识包 | READY（已整理，待导入/直推 IMA） | ⚠️ IMA MCP 连接器未连接，文件已就绪，连接后直推或手动导入两机 |

## 🧩 可复用骨架（模板资产）

| # | 资产 | 路径 | 说明 |
|---|---|---|---|
| 4 | 发布文案骨架（R12+ 填变量） | `opc-doc/outputs/08-asset/r12-launch-template.md` | X/IH/PH/Reddit/GEO 五渠道结构与变量占位 |
| 5 | 产品工厂生成器 | `product-factory/make_product.py` | 配置驱动批量造 Next.js 微 SaaS |
| 6 | 真实 LLM 注入脚本 | `product-factory/set_llm_env.sh` | Vercel 注入 NVIDIA NIM 三变量 |

## 🔧 运营资产（流程）

| # | 资产 | 路径 | 说明 |
|---|---|---|---|
| 7 | 会话摘要 | `opc-doc/outputs/00-orchestrator/session-summary.md` | 全周期决策流水 |
| 8 | 状态机 | `opc-doc/state/current-stage.json` | 阶段 + 执行期状态 |

---

### 登记说明
- 本次（2026-07-12）新增 #1–#3 内容资产 + #4 模板资产，均为执行期「不依赖 KYB」并行产出。
- 唯一未登记项：47 款产品各自的落地页文案分散在 `12_Micro_SaaS出海/<slug>/lib/product.ts`，未汇总为独立内容资产；如需可后续抽 `product-copy-inventory.md`。
