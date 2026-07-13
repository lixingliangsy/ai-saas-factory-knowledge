# R11 三款胜者 · GTM 落地计划（aiactradar / agentredteam / privacyscan）

> 编制：转化路径设计师（opc-conversion-designer）｜日期：2026-07-12
> 依据：`STRATEGY_2026.md` §4（获客路径）、`round-11-review.md`（瓶颈=获客）、`r11-winners-selection.md`（选型）、`WAFFO_KYB_GUIDE.md`（收款解锁）
> 目标：以"实现真实销售 / 实际收入"为终极判据，打通 **上架 → 推广 → 转化** 闭环。先打 **aiactradar**（2026-08-02 EU AI Act 高风险义务生效，硬 deadline），agentredteam / privacyscan 同属治理/安全/隐私 SEO 簇，承接导流。

---

## 1. 前提解锁清单（Prerequisites · 第 0 步）

- **真实 LLM 部署（AI 可执行，需用户给 key）**：3 款当前跑 mock。部署时向 Vercel 注入 3 个环境变量——`OPENAI_API_KEY` / `OPENAI_BASE_URL=https://integrate.api.nvidia.com/v1` / `OPENAI_MODEL=meta/llama-3.1-8b-instruct`——触发生成器调用真实 NVIDIA NIM。注入后重部署，跑一次真实扫描验证输出非硬编码、可信。
- **Waffo 真实收款（需用户亲办）**：店铺现"审核中"、仅测试环境。按 `WAFFO_KYB_GUIDE` 走 KYB——个人/独立开发者即可通过（无需公司实体）：填 Business Details + 验证支持邮箱 + 建 `/privacy`、`/terms` 页 → Submit for review（1–3 工作日）→ Approved 后配 Profile 法定姓名 + 打款账户（银行卡/支付宝）。**这步只能用户亲自做，AI 出不了合规身份。**
- **落地页合规件**：3 款产品页须公开可访问、定价清晰可见、含隐私/条款页——属 KYB 审核前置，AI 可建页、用户给域名/邮箱。

## 2. 上架步骤（Waffo 翻 live）

1. 完成 §1 的 KYB，等 Approved。
2. 在 Waffo 按 `waffo_product_registry.json` 纪律建 3 款**订阅**产品，**价格严格锁定 $59 / $79 / $29（monthly）**，规避历史 SDK 价格 100× 异常重演；建后写入 registry。
3. 用 R11 已生成的 **test checkout 链接** 做 smoke：测试卡走完"发起→跳转→回执"全流程，确认按钮/回调/金额正确（KYC 前后均可做，不阻塞主线）。
4. Approved 后翻 production；首单用真实卡再走一遍收口验证。

## 3. 营销推广（聚焦 aiactradar 先打，其余导流）

- **主渠道（1–2 个，不铺量）**：① X(Twitter) + IndieHackers 周更 Build-in-Public（AI 治理 builder 人设，数据截图抢算法）；② GEO/SEO 内容页（写"EU AI Act compliance checklist 2026 / obligation timeline"等搜索意图词，借已建 GEO 引擎）。
- **辅助软性**：Reddit 细分版（r/EU_AI_Act、r/AI_Agents、r/compliance）真实参与答疑、顺带亮产品，不 spam；aiactradar 首发 Product Hunt。
- **4 周节奏**：W1 上线 + 翻 live + PH 首发 + 首条 X；W2–W4 每周 2 条 X + 1 篇 GEO 长文（agentredteam / privacyscan 各 1 篇承接导流）。其余 5 款 R11 暂不投流，留作 SEO 长尾。

## 4. 转化路径（触达 → 承接 → 成交）

- **路径**：GEO / X 内容 → Vercel 落地页（价值一句 + 价格 + 主 CTA）→ 免费扫描 / 邮箱留资（承接）→ Waffo 结账（成交）→ 订阅 + 同簇交叉推荐（回流）。
- **埋点（GA4 + Waffo）**：`page_view` → CTA 点击 `generate_lead` → Waffo `begin_checkout`（带 UTM）→ `purchase`（Waffo 回跳 / Webhook）。周度看"访问 → checkout"转化率漏斗。
- **首单钩子**：上线首月"首月 7 折"降低首付摩擦；年付"送 2 个月"（付 10 得 12）抬 LTV。三款捆绑"AI 治理套件"年付折上折，促交叉转化。

## 5. 里程碑与 Done 标准（对齐 round-11-review）

| 周 | 检查点 | Done 标准 |
|---|---|---|
| W1 | 上架 + 首发 | KYB 提交 / Approved；3 款 live；PH 发布；test checkout smoke 通过 |
| W2 | 起量 | ≥50 合格访客；≥1 次 checkout 尝试（test / live 均可） |
| W3 | 信号 | ≥150 访客；识别哪款点击 / 意图胜出 |
| W4 | 收入 | 周期累计 ≥200–500 访客；**首笔真实付费订阅**到账 |

> 终极 Done：拿到第一个需求信号 + 第一笔真实收入（哪怕 $29）。**流量 = 0，则其余全为 0**——本周期成败以需求信号为准，不以付款开关为准。

## 6. 风险与回退

- **KYC 延迟**：不空等——用 test checkout 链接提前跑转化话术 / 落地页 A/B，验证"访客 → checkout"话术是否成立；Approved 即切真收款。
- **流量不及预期**：GEO 长尾词（"EU AI Act high-risk obligations August 2026"）低成本起量，不买量；加投 Reddit 细分版真实答疑；若 4 周仍近 0 访问，回退换渠道（HN / Dev Twitter / 合规 newsletter）。
- **真实 LLM 不稳**：保留 mock 兜底页 + "生成中"提示，避免付费后空结果；先小流量验证输出质量再放大投流。
