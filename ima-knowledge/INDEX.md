# 一人公司项目 · IMA 共享知识库（主场 + 副机协同）

> 整理日期：2026-07-13 ｜ 主理人：易牧（OPC 一人公司专家团）
> 用途：本机与另一台并行推进的电脑共享项目知识，避免重复造产品 / 重复造技能 / 重复决策。

## 项目身份（已定）
AI 编程驱动的**欧美微 SaaS / App 工厂**（创始人 李星亮 博士）。
- 打法：AI 编程工具批量造 Next.js 产品 → Vercel 部署 → Waffo/Stripe 订阅 → AI 内容 + X/IH/GEO 获客。
- 赚会员订阅，主攻欧美。
- ⚠️ 早期 OPC 建盘期曾把利基定为「科研图表」，已被推翻；真实身份 = 「AI 编程驱动、面向欧美的微 SaaS 工厂」。

## 当前状态（2026-07-13）
- ✅ Waffo KYB **已批准**（2026-07-13），店铺状态「已审核」——解锁上架与真实收款。
- ✅ 约 71 款英文产品部署在 Vercel，**全部注入真实 NVIDIA NIM LLM**（VERCEL REST 实测确认）。
- ✅ 44 款已上架 Waffo（测试态 checkout 可用）。
- ✅ R11 三款赢家（aiactradar/agentredteam/privacyscan）营销文案的 `[LIVE_SOON]` 占位已替换为真实 checkout 链接（带渠道 UTM）。
- 🔄 R14（HR-tech 8 款）、R15（PropTech 8 款）已部署；Waffo 补列中。
- 🔄 gtag.js / GA4 接线进行中（缺 GA4 Measurement ID，见 decisions-log）。

## 副机协同约定
另一台电脑并行推进，**侧重点不同**。请：
1. 开新产品的**第一步先查 `products-catalog.md`**，确认 slug 未重复、品类未重叠。
2. 需要新能力前先查 `skills-inventory.md`，避免重复造技能。
3. 做任何产品/定价/合规决策前查 `decisions-log.md` 与 `conventions.md`。

## 文件索引
| 文件 | 内容 |
|---|---|
| `products-catalog.md` | 全部已部署产品的 slug / 名称 / 品类 / 价格 / Waffo 上架状态（防重复造同款） |
| `skills-inventory.md` | 已沉淀的技能与关键 SOP（防重复造技能） |
| `conventions.md` | 产品硬规则、定价数学、真实 LLM 注入 SOP、Waffo 上架 SOP、工具链 |
| `decisions-log.md` | 关键决策与待办（防重复决策） |

## ⚠️ 待办 / 阻塞（需人工）
1. **IMA 连接器未连接**：本会话 IMA MCP 连接器为 disconnected，无法直接推送到 IMA 知识库。本目录 `IMA知识库/` 已是整理好的结构化文档，请在 WorkBuddy 设置中连接 IMA 连接器后由主理人直推，或手动导入这两台电脑的 IMA。
2. **生产态真实收款**：当前 Waffo checkout 为测试态（pancake.waffo.ai）。KYB 已过后要让链接真实扣款，需把店铺切到生产态并生成生产态 checkout 链接（涉及真实资金，建议用户在 Waffo Dashboard 确认切换方式后再批量替换）。
3. **GA4 Measurement ID 缺失**：gtag 接线已完成但无 `G-XXXXXXX`，分析无数据。需用户提供服务端 ID。
