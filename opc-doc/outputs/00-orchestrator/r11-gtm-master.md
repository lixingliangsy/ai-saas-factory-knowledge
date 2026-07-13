# R11 胜者 · 真实销售推进总计划（Master Plan）

> 编制：主理人 易牧（OPC 专家团）｜日期：2026-07-12
> 输入：林利基（选型评分）+ 言之（价值主张）+ 闭环（GTM 落地计划）+ 联网 2026 市场信号
> **终极目标：实现真实销售、产生实际收入，而非停留在模拟/自测。**

---

## 一、选型结论与依据（回答"为什么是这 3 个"）

用六维加权框架（市场 25% / 付费 20% / 差异 15% / 真实化 15% / 获客 15% / 销售契合 10%）对 R11 八款评分：

| 产品 | 价格 | 总分 | 一句话依据 |
|---|---|---|---|
| **aiactradar** | $59 | **4.40** | EU AI Act 2026-08-02 高风险义务硬 deadline；SMB 合规成本 €200k–€500k，$59 ROI 极硬；真实 LLM 可生成合规映射 |
| **agentredteam** | $79 | **4.40** | 2026 赛道融资火热（AI 安全 $374M、红队 $207M，Promptfoo 被 OpenAI 收）；买家视红队为基础设施 |
| **privacyscan** | $29 | **3.95** | GDPR/AI Act 叠加的轻量 SMB 需求，$29 B2B 可持续档，同簇可导流 |
| medguard | $49 | 3.30 | 医疗合规重、验证慢 |
| kidprivacy | $9 | 2.95 | 消费端 $9 付费意愿弱 |
| geovisor | $39 | 2.70 | 差异化低、获客模糊 |
| litwatch / tutorguard | $9 | 1.95 | 消费端，付费与获客皆不利 |

**单推首选 = aiactradar**（"为什么是现在"最硬、B2B ROI 最易讲清、资金型 incumbent 少于红队赛道）。

**市场信号支撑**（联网核实 2026-07-12）：Gartner AI 治理平台市场 ~$492M、CAGR >28%；AI 安全/红队 2026 融资 $374M/$207M；EU AI Act 罚款 €35M 或 7% 全球营收。三款同属治理/安全/隐私 SEO 簇，可打包成"AI 治理套件"互相导流。

---

## 二、价值主张（回答"卖点怎么讲"，已对齐真实 LLM 能力）

| 产品 | Tagline | ROI 钩子 |
|---|---|---|
| AIActRadar | "Map your AI to its EU AI Act obligations in minutes." | 罚款 €35M/7% 全球营收；<$59/月 ≈ 一小时律师费 |
| PrivScan | "Catch GDPR gaps on your site before regulators do." | GDPR 罚款 €20M/4% 营收 |
| AgentRedTeam | "Break your AI agents before attackers do." | IBM 2024 数据泄露均值 $4.9M |

准确性护栏：AIActRadar 生成合规映射/报告（不替代律师）；AgentRedTeam 跑自动化漏洞扫描（不保证不被攻破）；PrivScan 扫隐私差距（不发认证）。完整英文落地页首屏文案见 `03-value/r11-winners-value-props.md`。

---

## 三、落地推进计划（回答"怎么上架→推广→转化"）

### 第 0 步 · 两个前置解锁（缺一不可）
1. **真实 LLM 部署**：向 Vercel 注入 `OPENAI_API_KEY`(NVIDIA NIM nvapi-…) / `OPENAI_BASE_URL=https://integrate.api.nvidia.com/v1` / `OPENAI_MODEL=meta/llama-3.1-8b-instruct`，重部署 3 款，跑一次真实扫描验证非硬编码。
   - ⚠️ 当前阻塞：`next build` 被沙箱判为 bulk-delete（已 3 次拒）。需用户在本地或放行沙箱外执行 `product-factory/pf_build_deploy.py` + `product-factory/set_llm_env.sh`。
2. **Waffo KYB（真实收款）**：只能用户亲办（独立开发者即可，无需公司）。填 Business Details + 验邮箱 + 建 `/privacy` `/terms` → 提交审核（1–3 工作日）→ Approved 后配打款账户。

### 上架（Waffo 翻 live）
- KYB Approved 后，按 `waffo_product_registry.json` 纪律建 3 款**订阅**产品，**价格严格锁 $59/$79/$29**（规避历史 100× bug）。
- 用 R11 已生成的 test checkout 链接先 smoke（不阻塞主线）；Approved 后翻 production，真实卡收口验证。

### 推广（只押 1–2 渠道，先打 aiactradar）
- **主**：X + IndieHackers 周更 Build-in-Public；GEO/SEO 长文（"EU AI Act compliance checklist 2026" 等意图词）。
- **辅**：Reddit 细分版（r/EU_AI_Act、r/AI_Agents、r/compliance）真实答疑；aiactradar 首发 Product Hunt。
- **4 周节奏**：W1 上线+PH；W2–W4 每周 2 条 X + 1 篇 GEO 长文（agentredteam/privacyscan 各承接导流）。

### 转化路径 + 跟踪
- 路径：内容 → Vercel 落地页 → 免费扫描/留资 → Waffo 结账 → 订阅 + 同簇互导。
- 埋点：GA4 `page_view→generate_lead→begin_checkout→purchase` + Waffo 回跳，周度看"访问→checkout"漏斗。
- 钩子：首月 7 折；年付送 2 个月；三款"AI 治理套件"年付折上折。

---

## 四、里程碑与 Done 标准（持续跟踪用）

| 周 | 检查点 | Done 标准 |
|---|---|---|
| W1 | 上架+首发 | KYB 提交/Approved；3 款 live；PH 发布；test checkout smoke 通过 |
| W2 | 起量 | ≥50 合格访客；≥1 次 checkout 尝试 |
| W3 | 信号 | ≥150 访客；识别胜出款 |
| W4 | 收入 | 累计 200–500 访客；**首笔真实付费订阅到账** |

> 终极判据：**拿到第一个需求信号 + 第一笔真实收入（哪怕 $29）**。流量=0 则其余全为 0——本周期成败以需求信号为准。

## 五、风险与回退
- KYB 延迟：用 test checkout 提前验证转化话术/落地页，不空等。
- 流量不及预期：GEO 长尾词起量不买量；4 周近 0 访问则换 HN/Dev Twitter/合规 newsletter。
- 真实 LLM 不稳：保留 mock 兜底页 + "生成中"提示，先小流量验质量再放大。

---
*本计划为 R11 治理簇的权威 GTM，与旧 `07-conversion/conversion-path-draft.md`（科研利基草案）并存不冲突。所有专业结论均来自对应 OPC 成员产出，主理人仅做编排汇编。*
