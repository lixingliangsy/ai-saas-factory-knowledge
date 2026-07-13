# R11 Week-1 发布内容包 + GA4 埋点方案

> **编制**：转化路径设计师（opc-conversion-designer）｜日期：2026-07-12
> **依据**：`r11-winners-gtm-plan.md` §4（转化路径）、`r11-winners-value-props.md`（真实价值锚点）、`r11-gtm-master.md`（里程碑）
> **范围声明（重要）**：本文件交付「转化闭环设计师」职责范围内的部分 ——
> - **A 部分 = 渠道与内容触达结构（Channel & Content Brief）**：定义每款产品 × 每渠道的**角色、价值锚点、CTA 原语、驱动事件、内容角度**。这是「内容触达方式类别」，并非可复制的英文文案正文。
> - **B 部分 = GA4 测量框架（Measurement Framework）**：事件体系、dataLayer 结构、漏斗与预警阈值、dashboard 指标、Next.js 接线位置。这是「闭环测量 / 路径假设与测量指标」。
>
> **边界（依 `opc-conversion-loop` 技能与角色定义）**：
> - ❌ 具体可发布英文文案（Task A 正文，X/IH/PH/Reddit/GEO 的成稿）属**内容资产阶段（opc-asset-ops / opc-asset-strategist）**——已按本文件「内容简报」转交资产策略师撰写。
> - ❌ 生产级 gtag.js 接线与错误处理（consent mode、server-side MP、重试）属**工程实现**——本文件给出事件设计与接线位置，由工程按此落地。
>
> **前置纪律（KYB 未过）**：所有发布链接用占位 `[LIVE_SOON]`，CTA 一律走「候补名单 / 抢早期鸟价」，**不写 buy now**；待 KYB Approved 仅替换链接与目标事件权重（generate_lead → begin_checkout 提升）。

---

## A. 渠道与内容触达结构（Channel & Content Brief）

### A.0 闭环骨架（继承自 GTM §4，已确认）
```
触达: X / IndieHackers / Product Hunt / Reddit / GEO 目录
  ↓
承接: Vercel 落地页（一句话价值 + 价格 + 主 CTA）→ 免费扫描 / 邮箱留资
  ↓
成交: Waffo 订阅结账（$59 / $79 / $29 monthly）
  ↓
回流: 同簇交叉推荐（AIActRadar ↔ AgentRedTeam ↔ PrivScan）
```

### A.1 优先级与首打产品
- **首打 = AIActRadar**（$59）：2026-08-02 EU AI Act 高风险义务硬 deadline，「为什么是现在」最硬，B2B ROI 最易讲清。
- **承接导流**：AgentRedTeam（$79）、PrivScan（$29）同属「AI 治理 / 安全 / 隐私」SEO 簇，在 W2–W4 各 1 篇 GEO 长文承接导流，结果/结账页互链。

### A.2 每产品 × 每渠道简报（内容角度，非文案）

> 说明：下表「内容角度」是**写什么、为什么**，给资产策略师的撰写简报；「驱动事件」是 GA4 应归因的事件（见 B 部分）。具体成稿由 opc-asset-strategist 产出。

#### 通用 CTA 纪律（KYB 前）
- 文案 CTA 一律：`Join the waitlist` / `Lock early-bird pricing` / `Get notified at launch`
- 链接占位 `[LIVE_SOON]`，统一 UTM：`utm_source={渠道}`、`utm_medium={paid|organic|referral}`、`utm_campaign=r11_launch`、`utm_content={产品}`
- KYB Approved 后：仅替换 `[LIVE_SOON]` 为真实 Waffo 结账链接；`generate_lead` 仍是主事件，但 `begin_checkout`/`purchase` 才开始有量。

#### AIActRadar（$59 · 首打）
| 渠道 | 角色 | 价值锚点（来自 value-props） | 内容角度（非文案） | 驱动事件 |
|---|---|---|---|---|
| X / Twitter | 触达 | 2026-08-02 硬 deadline；€35M/7% 罚款；$59 < 1 小时律师费 | Launch 主帖：deadline + ROI 对比；Thread：用真实一次扫描截图演示「风险分级→义务清单→行动项」 | `generate_lead` |
| IndieHackers | 触达+信任 | 真实 LLM 生成合规映射（非模板） | Build-in-Public：为什么建、一次真实扫描的故事、截图、邀测 | `generate_lead` |
| Product Hunt | 触达（首发） | 分钟级出 EU AI Act 义务清单 | Launch 标题+tagline+首评（讲「为什么是现在」+ 早期鸟） | `view_item` → `generate_lead` |
| Reddit | 触达（价值优先） | 风险分级 / 义务映射 | r/EU_AI_Act、r/compliance：答疑+亮产品，不硬广；软 CTA 指向落地页 | `page_view`（referral） |
| GEO 目录 | 收录/承接 | 一句话收录描述 | There's An AI For That / Futurepedia / AI Agents Directory：简洁收录 | `view_item` |

#### AgentRedTeam（$79）
| 渠道 | 角色 | 价值锚点 | 内容角度 | 驱动事件 |
|---|---|---|---|---|
| X / Twitter | 触达 | $4.9M 均值泄露；在攻击者前攻破自己的 Agent | Launch：可 Exploitability 评分 + Top-3 攻击向量示例；Thread：一次红队演示 | `generate_lead` |
| IndieHackers | 触达+信任 | 真实自动化漏洞扫描（不保证不被破） | BiP：builder 视角的 agent 安全风险故事 + 扫描截图 | `generate_lead` |
| Product Hunt | 触达（W2+） | 红队即基础设施 | Listing：把红队做成可持续习惯 | `view_item` → `generate_lead` |
| Reddit | 触达 | 攻击向量（prompt injection via tool output 等） | r/AI_Agents、r/programming：技术答疑+真实向量示例 | `page_view` |
| GEO 目录 | 收录 | 一句话收录 | AI Agents Directory 等 | `view_item` |

#### PrivScan（$29）
| 渠道 | 角色 | 价值锚点 | 内容角度 | 驱动事件 |
|---|---|---|---|---|
| X / Twitter | 触达 | €20M/4% 罚款；$29 找 consent/disclosure 缺口 | Launch：0–100 隐私分 + 文章级问题；Thread：一次扫描演示 | `generate_lead` |
| IndieHackers | 触达+信任 | 无 DPO 也能做隐私体检 | BiP：indie 开发者的 GDPR 踩坑 + 扫描 | `generate_lead` |
| Product Hunt | 触达（W3+） | 在监管前发现缺口 | Listing：轻量隐私体检 | `view_item` → `generate_lead` |
| Reddit | 触达 | 文章级问题（Art.7 细粒度同意等） | r/privacy、r/compliance：答疑+亮产品 | `page_view` |
| GEO 目录 | 收录 | 一句话收录 | Futurepedia 等 | `view_item` |

### A.3 同簇互导（回流测量）
- 落地页 / 结果页底部加「相关工具」区块，互链另两款，链接带 `utm_content=cross_{source}`。
- GA4 用 `crossing_link` 自定义事件（或 `view_item` 的 `item_list="cross_promo"`）追踪互导点击。

> ✅ **Task A 边界处理**：以上为「内容简报」。X/IH/PH/Reddit/GEO 的**可复制英文成稿**已由本文件转交 `opc-asset-strategist` 依此撰写（锚定 value-props 真实价值、语气地道不夸大、CTA 用候补名单/早期鸟、链接 `[LIVE_SOON]`）。本阶段不产出具体文案正文。

---

## B. GA4 埋点测量框架（Measurement Framework）

> 目的：把「访客 → 试用 → 留资 → 结账 → 付费」闭环变成可度量、可预警的信号，对齐 GTM 里程碑（W2 ≥50 合格访客；W4 首笔真实付费）。

### B.1 事件体系（触发时机 + 参数）

| 事件 | 触发时机 | 关键参数 | 备注 |
|---|---|---|---|
| `page_view` | 落地页 / 结果页每次加载 | `page_title`, `page_location`, `utm_*` | gtag config 自动收集 |
| `view_item` | 定价卡 / PH listing / GEO 收录页 / 交叉推荐区块曝光 | `currency=USD`, `items[]`（`item_id`,`item_name`,`price`,`item_variant`） | 衡量「看到报价」 |
| `generate_lead` | 点击候补名单 / 早期鸟 CTA（**KYB 前主转化事件**） | `currency`, `value`（首月折扣价）, `items[]` | 需求信号核心 |
| `sign_up` | 邮箱留资提交成功（免费扫描需邮箱时） | `method="email"` | 承接成功 |
| `tool_used` *(自定义)* | `/api/tool` 真实扫描成功返回 | `item_id`, `success=true/false`, `llm_real=true/false` | 衡量「试用」环节 + 真实 LLM 成功率护栏 |
| `begin_checkout` | 点击 Waffo 结账（KYB 后） | `currency`, `value`, `items[]`, `coupon` | 成交启动 |
| `add_payment_info` | Waffo 支付信息页到达 | `payment_type` | 结账摩擦点 |
| `purchase` | Waffo 回跳 / Webhook 确认订阅 | `transaction_id`, `currency=USD`, `value`, `items[]`, `subscription=true` | 北极星达成 |
| `share` | 结果页 / 报告分享 | `method`, `content_type` | 口碑传播 |

**参数规范**
- `currency` 固定 `USD`；`value` 用「该次动作对应金额」：generate_lead 用首月折扣价（如 AIActRadar 7 折 ≈ $41.30），purchase 用实际订阅金额。
- `items[]` 统一结构：`{ item_id:"aiactradar", item_name:"AIActRadar", price:59, item_variant:"monthly" }`。
- 所有事件携带 `utm_source / utm_medium / utm_campaign=r11_launch / utm_content={产品|cross_源}`，用于渠道归因。

### B.2 dataLayer 结构（事件设计的具体形态）

```js
// 标准 ecommerce items 结构
window.dataLayer = window.dataLayer || [];

// 清空上一笔 ecommerce，避免叠加
dataLayer.push({ ecommerce: null });

// 例：候补名单点击（KYB 前主事件）
dataLayer.push({
  event: "generate_lead",
  ecommerce: {
    currency: "USD",
    value: 41.30,                 // AIActRadar 首月 7 折
    items: [{ item_id: "aiactradar", item_name: "AIActRadar", price: 59, item_variant: "monthly" }]
  }
});

// 例：真实扫描成功（试用环节）
dataLayer.push({
  event: "tool_used",
  tool: { item_id: "aiactradar", success: true, llm_real: true }
});

// 例：订阅付费（KYB 后，通常由服务端 MP 推送）
dataLayer.push({
  event: "purchase",
  ecommerce: {
    transaction_id: "waffo_<order_id>",
    currency: "USD",
    value: 59.00,
    subscription: true,
    items: [{ item_id: "aiactradar", item_name: "AIActRadar", price: 59, item_variant: "monthly" }]
  }
});
```

> **接线归属**：以上 `dataLayer.push` 形状是「测量设计」的 concrete form，可直接给工程落地。生产级 `gtag.js` `<script>` 初始化、Next.js `_document`/`layout` 注入、Consent Mode、服务端 Measurement Protocol（purchase 防丢）属**工程实现**，依此形状接线。

### B.3 漏斗定义 + 预警阈值

```
访客 (page_view)
  → 试用 (tool_used / /api/tool 调用)
    → 留资 (generate_lead / sign_up)
      → 结账 (begin_checkout)
        → 付费 (purchase)
```

| 环节 | 转化目标 | 预警阈值（跌破即告警） | 含义 |
|---|---|---|---|
| 访客 → 试用 | ≥40% 落地页访客触发 `/api/tool` | <15% | 落地页价值传达弱 / 试用门槛高 |
| 试用 → 留资 | ≥20% 试用者留资 | <5% | 钩子或信任不足 |
| 留资 → 结账 | ≥10%（KYB 后） | <3% | 价格 / 结账摩擦 |
| 结账 → 付费 | ≥3%（冷流量订阅） | <1% | 价格锚定 / 支付失败 |

**对齐里程碑**：W2 ≥50 合格访客且 ≥1 次 checkout 尝试；W3 ≥150 访客识别胜出款；W4 累计 200–500 访客 + 首笔 `purchase`。终极判据：**首个需求信号（generate_lead）+ 首笔真实收入（purchase，哪怕 $29）**。流量=0 则其余全为 0。

### B.4 Dashboard 指标清单

**北极星指标（North Star）**
- KYB 前：`generate_lead` 周增量（需求信号强度）
- KYB 后（切换）：`purchase` 周增量 = **付费订阅数**；长期北极星 = **MRR**（月经常性收入）

**护栏指标（Guardrails）**
1. 漏斗各环节转化率（须高于 B.3 阈值）
2. 渠道质量分布（`utm_source` 来源占比 + 各源 generate_lead→purchase 率）—— 识别胜出渠道，避免买量
3. 真实 LLM 成功率（`tool_used.llm_real=true` 占比 ≥95%）—— 护栏：mock 兜底率过高说明真实部署不稳
4. 候补名单 → 付费 转化率（验证需求信号质量）
5. 退款 / 投诉率（护栏，订阅业务健康度）

### B.5 接线位置（给工程）

| 事件 | 接线点 |
|---|---|
| `page_view` | gtag config 自动（落地页 layout） |
| `view_item` | 定价卡 / 交叉推荐区块曝光（IntersectionObserver） |
| `generate_lead` | CTA 按钮 `onClick` → `dataLayer.push` + `gtag('event', ...)` |
| `tool_used` | `/api/tool` 成功返回后前端 push（含 `llm_real`） |
| `begin_checkout` / `add_payment_info` | Waffo 结账跳转 / 支付页到达 |
| `purchase` | Waffo 回跳 URL 参数 + **服务端 Webhook → GA4 Measurement Protocol**（防前端丢失） |
| `share` | 分享按钮 `onClick` |

---

## C. 交付边界与下一步（Handoff）

| 模块 | 归属 | 状态 |
|---|---|---|
| A 渠道与内容简报（角度/CTA/事件） | 转化闭环设计师（本文件） | ✅ 已交付 |
| B GA4 测量框架（事件/dataLayer/漏斗/指标/接线） | 转化闭环设计师（本文件） | ✅ 已交付 |
| **A 具体英文成稿（X/IH/PH/Reddit/GEO ×3）** | **opc-asset-strategist（内容资产阶段）** | ⏳ 待资产策略师依简报撰写 |
| B 生产级 gtag.js 接线 + 服务端 MP | 工程实现 | ⏳ 依本框架接线 |

**即拿即发条件**：KYB Approved 后，资产策略师交付的文案成稿 + 本框架的 GA4 接线 = 可一键点火。当前（KYB 前）两件套均处「待填充链接 `[LIVE_SOON]`」状态，不阻塞主线。

*本文件与 `r11-winners-gtm-plan.md` §4 一致；为 R11 治理簇 Week-1 点火的测量与触达结构权威底稿。*
