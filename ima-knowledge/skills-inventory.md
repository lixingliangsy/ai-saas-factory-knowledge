# 技能清单（Skills Inventory）

> 已沉淀在 user 级 `~/.workbuddy/skills/`。副机若需相同能力，先确认是否已存在，避免重复造。
> 路径：`C:/Users/Administrator/.workbuddy/skills/<name>/SKILL.md`

## 核心生产技能

### product-factory-round（v2.1.0）
批量造英文微 SaaS 的 data-driven 流程。关键 SOP：
- **§11 趋势筛选**：每轮必调 `last30days` 评估近 30 天增长趋势；但 `last30days` 在无人值守沙箱为**交互式**（需 AskUserQuestion）不可跑 → 已授权**回退 WebSearch**（"rising search trends 2026 micro SaaS <niche>"）评估搜索量上升趋势。
- **§12 部署后必注入真实 NVIDIA LLM**：用 Vercel REST API `POST /v9/projects/{slug}/env` 注入 `OPENAI_API_KEY`/`OPENAI_BASE_URL`/`OPENAI_MODEL`，再 `vercel deploy --prod` 重建。
- **§13 中文文案必过 de-ai-copy**：降 AI 痕迹、口语化。

### waffo-product-listing（v1.0.0）
Waffo 上架（测试态，已验证流程）。关键：
- 测试版本由 **TEST 私钥** `create()` 铸造（不是 Dashboard Test Mode 开关）。
- `401/429` 网关限流 → 3s 间隔 + 指数退避重试（瞬时，可恢复）。
- 价格格式：字符串小数 `"19.00"`，非分、非数组；`billingPeriod: monthly`。
- 无顶层 `category`，放 `metadata.category`。
- checkout 链接：`client.checkout.anonymous.create({productId, currency:'USD'})` → `pancake.waffo.ai/store/.../checkout/cs_xxx`。
- 注册表 `waffo_product_registry.json` 防重 + 防价格漂移（validatePrice 上限 $200）。

### de-ai-copy（v1.0.0）
去 AI 痕迹。手法：口语填充词 / 长短句交错 / 真实个人语气 / 去统一结构 / 具体细节 / 容错。**反模式**：只换同义词不算去 AI。

## 辅助 / 方法论技能
- `last30days`：交互式社交监听引擎（沙箱不可用，已回退 WebSearch）。
- `micro-saas-market-eval`：产品市场评估（评分排序）。
- `vercel-deploy-reconcile`：部署后环境变量校验（REST 实测）。
- `opc-orchestrator` / `opc-team-mode`：OPC 一人公司 9 阶段方法论。
- `opc-resource-audit` / `opc-niche-positioning` / `opc-value-proposition` / `opc-business-model-design` / `opc-mvp-designer` / `opc-conversion-loop` / `opc-asset-ops` / `opc-dashboard-review`：各阶段成员技能。

## 副机注意
- 若副机要造新产品，直接用 `product-factory-round` v2.1 流程，**不要另起一套生成器**。
- 若 `last30days` 在副机环境可交互运行（有人值守），则按原流程调用，不必回退 WebSearch。
