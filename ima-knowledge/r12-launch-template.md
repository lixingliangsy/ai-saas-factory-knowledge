# R12 发布文案模板（可复用骨架 · 填变量即生成）

> 源自 `r11-week1-launch-copy.md` §7，扩展为可直接填表的模板。
> 适用：R12+ 任意新产品首发 Week-1。链接统一 `[LIVE_SOON]` 占位（KYB 前），KYB 后替换为真实 Waffo 结账链接。

---

## 0. 填充变量表（先填这个）

| 变量 | 取值 | 示例（R11） |
|---|---|---|
| `{PRODUCT}` | 产品名 | AIActRadar |
| `{PRICE}` | 月价 | $59 |
| `{TAGLINE}` | 一句话定位 | EU AI Act compliance, mapped in minutes |
| `{价值锚点}` | 核心价值一句 | 把你的 AI 系统映射到具体合规义务 |
| `{护栏句}` | 诚实边界声明 | 是地图，不是律所，不替代律师 |
| `{互导目标}` | 同簇互导产品 | AgentRedTeam |
| `{CTA}` | 行动号召 | Join the waitlist / Lock early-bird |
| `{SCREENSHOT}` | 截图占位标注 | [SCREENSHOT: 风险分级结果卡] |
| `{EMOJI}` | 主题 emoji | 🇪🇺 |

---

## 1. X / Twitter — Launch tweet
```
{EMOJI} {价值锚点}. {TAGLINE}. {PRICE}/mo. {CTA}: [LIVE_SOON]
```
护栏：≤280 字符；无绝对化词（best/perfect/guaranteed/#1）；无外链。

## 2. X / Twitter — Thread（6 条）
1. 钩子（deadline / 痛点）
2. 为什么建（个人故事）
3. {SCREENSHOT}
4. 能力映射（输入→输出）
5. 产出 + {护栏句}
6. ROI + {CTA} + 互导（见 {互导目标}）

## 3. IndieHackers — Build-in-Public
- 标题：`Building {PRODUCT}: {建X不建Y}`
- 故事（痛点）→ 3 bullet 能力 → {护栏句} → 早期鸟+候补 → 互动问句

## 4. Product Hunt — Listing（首发）
- Title: `{PRODUCT}`
- Tagline: ≤60 字符
- One-liner: ≤80 字符
- First comment: maker 自介 + {护栏句} + 早期鸟 30% off 首月

## 5. Reddit — 两帖（价值优先，不硬广）
- 标题：经验/问题式（含目标 sub）
- 正文：150–250 词，分享做法 + 软 CTA，首句无外链
- 推荐 sub：与产品领域相关（如 r/compliance / r/privacy / r/AI_Agents）

## 6. GEO 目录 — Listing
```
{PRODUCT} — {TAGLINE}。{核心能力一句}。{目标人群}。
```

---

## 7. 同簇互导 UTM 约定
```
[LIVE_SOON]?utm_source={渠道}&utm_medium=referral&utm_campaign=r12_launch&utm_content=cross_{source}
```
来源产品 → 互导目标，落 X thread 末条 / Reddit 软 CTA / 结果页「相关工具」区块。

## 8. 文案级合规护栏（每篇发前自检）
- [ ] 无绝对化词
- [ ] 法定数字仅作引用，非结果承诺
- [ ] {护栏句} 在
- [ ] 无真实外链，仅 `[LIVE_SOON]`
- [ ] CTA 统一候补名单/早期鸟，无 buy now

> 本模板为资产化产出，配合 `r11-week1-content-ga4.md` §B（GA4 事件体系）使用：CTA→`generate_lead`，互导→`utm_content=cross_{source}`。
