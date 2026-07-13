# R11 开发过程复盘 + 小红书种草文案（资产沉淀 · 沉墨）

> 产出：资产沉淀师 沉墨（opc-asset-strategist）
> 撰写：2026-07-13 ｜ 上下文：工厂第 11 轮已发布 8 个产品，累计 47 个英文产品 / Waffo 测试环境已发布 44 个
> 定位：本文件是**可复用资产文档**（A 部分）+ **对外引流文案草稿**（B 部分）。A 面向"想复制这条路的人"，信息密度高、可照抄；B 是小红书种草文案，含平台合规处理。
> 一句话：一个人 + AI 编程 + OPC 框架，半年攒 47 个英文微 SaaS——核心是把"造产品"变成可复用的工厂，把"挑赢家"交给数据。

---

# A 部分｜开发过程复盘（可复用资产文档）

## 0. 这份文档怎么用

照抄顺序：第 1 节确认你是不是同一类人 → 第 2 节照搬方法论 → 第 4 节对齐技术栈 → 第 5 节提前避坑 → 第 6 节直接拿 Checklist 开干 → 第 7 节看当前进度别重复造轮。

---

## 1. 背景与目标

- **角色**：独立开发者 / 一人公司（OPC，One-Person Company）。无团队、无融资、无雇员。
- **目标**：批量生产低成本、面向欧美的**英文**微 SaaS，靠**订阅**产生真实收入。
- **市场**：欧美（英文产品 + GEO/SEO 获客），不做中文市场。
- **为什么走这条路**：单人产能有限 → 用 AI 编程放大"造"的产能；用"批量出生、重点养 2–3"降低单点失败风险，不把希望押在单个产品上。
- **当前阶段**：已跨过"能不能造出来、能不能上线"的关卡，卡在"真实收款资质（KYB）"这一人为闸——这是本路线目前唯一没被工程化解决的点。

---

## 2. 方法论

### 2.1 AI 编程工厂（生产侧：把"造产品"变成配置）

- **配置驱动生成**：`gen_roundN.py` + `make_product.build_one()`，加一个产品 = 改一份配置数据，不碰业务代码。
- **质量门禁**（构建卫生三件套）：无 CJK 残留、 `next build` 退出码为 0、slug / category 不重复。
- **部署**：`deploy_rN.sh` 逐产品捕获 Aliased URL（已修 R9/R10 串号 bug）。
- **上架**：`waffo_list_priority.mjs` 走测试私钥流 + 退避重试；`waffo_product_registry.json` 防重复上架 / 防价格漂移。
- **营销**：`gen_marketing_factory.py` 每产品生成 5 份文案；`reconcile_marketing_urls.py` 做 URL 对账。
- **可移植**：全部用 `ROOT` 相对路径，跨机可直接跑。

### 2.2 一人公司 OPC 框架（经营侧：把"做生意"变成循环）

- **循环**：利基选择 → 价值主张 → MVP → 转化闭环 → 资产沉淀 → 经营复盘。阶段 08/09 是运营循环，可多次触发。
- **战略**：批量出生、重点养 2–3。
- **获客**：AI 内容 + X / IndieHackers / GEO 长文 / Reddit 细分版（Build-in-Public 优先）。
- **真实 LLM 接入**：每个产品的 `/api/tool.ts` 走 OpenAI 兼容协议，注入 `OPENAI_API_KEY` / `OPENAI_BASE_URL` / `OPENAI_MODEL` 即由 mock 变真实可用——这是"批量造 AI 产品"的关键抽象。

### 2.3 利基选择六维框架（挑赢家用）

权重：市场 25% / 付费 20% / 差异 15% / 真实化 15% / 获客 15% / 销售契合 10%。

---

## 3. 关键里程碑（R1 → R11）

- **R1–R10**：累积约 39 个产品；期间修复 deploy 串号 bug（R9/R10）。
- **R11（2026-07，战略转向）**：从"科研利基草案"转攻 **AI 治理 / 安全 / 隐私** 实际销售簇。
  - 新增 8 个产品：`geovisor / aiactradar / privacyscan / agentredteam / litwatch / tutorguard / kidprivacy / medguard`
  - 全部 `build / deploy / publish / test-checkout` 通过；真实 LLM 注入后 8 款全部 `READY`。
  - 六维评分选 3 胜者：**aiactradar(\$59) > agentredteam(\$79) > privacyscan(\$29)**，均属 AI 治理/安全/隐私 SEO 簇，可打包成"AI 治理套件"互导。
  - **单推首选 aiactradar**：2026-08-02 是 EU AI Act 高风险义务硬 deadline，SMB 合规成本 €200k–€500k，\$59 的 ROI 最容易讲清。
- **累计**：47 个英文产品；Waffo 测试环境已发布 44 个。

> R11 八款线上地址（已验证 READY，供对照）：aiactradar.vercel.app / agentredteam-coral.vercel.app / privacyscan-ten.vercel.app / geovisor-two.vercel.app / litwatch.vercel.app / tutorguard.vercel.app / kidprivacy.vercel.app / medguard-mu.vercel.app

---

## 4. 技术栈与工具链

| 层 | 选型 | 备注 |
|---|---|---|
| 应用 | Next.js（App Router，serverless `/api/tool.ts`） | 每个 AI 能力挂一个 tool 端点 |
| 部署 | Vercel（全局 `vercel` 55，`vercel deploy --prod`） | 用 REST API 注入 env 更稳 |
| 收款 | Waffo 订阅（主）+ Stripe（备选） | `waffo_product_registry.json` 锁价，规避 100× bug |
| LLM | OpenAI 兼容协议 → NVIDIA NIM | 端点 `integrate.api.nvidia.com/v1`；模型 `meta/llama-3.1-8b-instruct`；key `nvapi-…`（NVIDIA NIM key） |
| 脚本链 | `gen_roundN.py` / `make_product` / `deploy_rN.sh` / `waffo_list_priority.mjs` / `gen_marketing_factory.py` / `reconcile_marketing_urls.py` / `switch_to_custom_domain.py` | 生产侧已是强资产 |
| 运行时 | 系统 Node 24（`npm.cmd`）+ 全局 `vercel` 55 | 托管 Node 22 的 npm 整套损坏，见 5.1 |

**真实 LLM 接入要点**：`/api/tool.ts` 用 OpenAI SDK 风格，只需三个环境变量即可从 mock 切真实：
```
OPENAI_API_KEY = nvapi-…          # NVIDIA NIM key
OPENAI_BASE_URL = https://integrate.api.nvidia.com/v1
OPENAI_MODEL = meta/llama-3.1-8b-instruct
```
验证链路正确判据：env 已注入 + 部署 `READY` + NVIDIA 端点此前已返回 HTTP 200 真实 completion。

---

## 5. 踩过的坑与解法（按出现频率排序）

| # | 坑 | 现象 | 根因 | 解法 |
|---|---|---|---|---|
| 5.1 | 托管 Node 损坏 | `npm` 命令整体失效 | 托管 Node 22.22.2 的 npm 根目录脚本指向不存在的 `npm-cli.js` | 改用系统 Node 24 的 `npm.cmd` + 全局 `vercel` 55 跑构建 |
| 5.2 | 沙箱拦截构建 | `next build` 重写 `.next` 被判定为 bulk-delete，多次拒绝 | 沙箱对批量写目录敏感 | 在沙箱外 / 本地执行 `product-factory/pf_build_deploy.py`；或放行沙箱 |
| 5.3 | 环境变量静默失败 | LLM 变量"看似注入"实则线上仍返回 mock | `set_llm_env.sh` 用 `vercel env add`，但脚本在非 *linked* 目录运行，每条静默失败（`>/dev/null 2>&1` 掩盖错误） | 改用 Vercel REST API `POST /v9/projects/{slug}/env` 注入，再 `vercel deploy --prod` 触发重建使变量生效 |
| 5.4 | 部署串号 | 多个产品 Aliased 到错误 URL | 批量部署时 slug/URL 串号 | `deploy_rN.sh` 逐产品捕获 Aliased URL（R9/R10 已修） |
| 5.5 | 价格漂移 | Waffo 上架价格错成 100× | 上架脚本无锁价纪律 | `waffo_product_registry.json` 锁价 + 严格纪律（R11 三胜者锁 \$59/\$79/\$29） |
| 5.6 | 线上无法自检 | 沙箱出网被拦，不能 `curl` 线上 `/api/tool` 确认真实返回 | 沙箱网络策略 | 以"env 注入 + READY + 端点已验证 200"作为链路正确判据，不依赖本机 curl |
| 5.7 | 真实收款闸 | 测试结账能跑，生产收款 \$0 | Waffo 店铺 KYB 需本人实名/资质 | **只能用户亲办**：Dashboard → Business Details + Payout Account 提交，1–3 天审核。AI 不可代做 |

**关键教训**：凡涉及"注入环境变量 / 跑 vercel 命令"，务必在 *linked* 项目目录内，或绕开 CLI 直接用 REST API；任何 `>/dev/null 2>&1` 掩盖错误的脚本都是定时炸弹。

---

## 6. 可复用经验 / Checklist

**工厂化清单（生产侧）**
- [ ] 加产品 = 改一份配置数据，不碰业务代码
- [ ] `ROOT` 相对路径，跨机可便携
- [ ] 质量门禁三件套：无 CJK / `build` 退出 0 / slug·category 不重复
- [ ] 上架用 registry 锁价 + 防重复

**真实 LLM 接入 SOP（避 5.3 坑）**
- [ ] 用 Vercel REST API `POST /v9/projects/{slug}/env` 注入三变量
- [ ] 注入后 `vercel deploy --prod` 触发重建使变量生效
- [ ] 判据：env 注入 + `READY` + 端点 200

**利基六维评分模板（挑赢家）**
- [ ] 市场 25% / 付费 20% / 差异 15% / 真实化 15% / 获客 15% / 销售契合 10%
- [ ] 批量出生、重点养 2–3；同簇可打包互导

**合规护栏话术（降低法律风险）**
- [ ] AIActRadar：生成合规映射/报告，**不替代律师**
- [ ] AgentRedTeam：跑自动化漏洞扫描，**不保证不被攻破**
- [ ] PrivScan：扫隐私差距，**不发认证**

**Build-in-Public 4 周节奏**
- [ ] W1 上线 + Product Hunt；W2–W4 每周 2 条 X + 1 篇 GEO 长文
- [ ] GA4 漏斗：`page_view → generate_lead → begin_checkout → purchase`

**优先沉淀资产路线图（来自 round-11 资产策略，供下一步）**
- A 跨产品分析总看板（数据资产，最高杠杆）
- B Waffo 生产态 KYC+切换 runbook（运维资产，收入硬前置）
- C 统一轮次编排脚本 `run_round.sh`（方法资产）
- D 产品深度/养育优先级清单（方法资产）
- E 聚类→SEO 中心/套件模板（内容/渠道资产）

---

## 7. 当前状态与下一步

- **状态**：47 个产品；Waffo 测试环境发布 44 个；R11 八款真实 LLM 接入 + `READY`。
- **唯一阻塞**：Waffo 店铺 KYB（真实收款），需用户亲办。
- **下一步**：
  1. 用户提交 KYB → 等 `Approved for production payments`
  2. 三胜者翻 live（锁 \$59/\$79/\$29）+ 跑真实 checkout 验证
  3. 点火 Week-1 发布内容包（X / IndieHackers / PH / Reddit / GEO）
  4. 4 周获客节奏 + GA4 漏斗监控
  5. 目标：W4 拿下首笔真实付费订阅
- **成败判据**：拿到首个需求信号 + 首笔真实收入（哪怕 \$29）。流量为 0 则其余全为 0。

---

# B 部分｜小红书种草文案（对外引流草稿 · 中文）

> 用途：在小红书分享"AI 编程批量做面向欧美的微 SaaS / 一人公司出海"的真实经历与干货，引独立开发者、产品经理、想搞副业出海的人。
> 基调：真诚分享、活人感、干货型（平台鼓励"真实生活记录"，打击虚假人设与精致摆拍）。
> 合规：已按 2026 小红书规范处理（见末尾自查清单）。本草稿为 AI 辅助创作，发布页须勾选「AI 辅助创作」。

## B.1 五个候选标题（≤20 字，前 16 字权重高）

1. 🤖 我造了47个出海小SaaS
2. 💡 一人公司出海，踩坑实录
3. 🛠️ 批量做欧美SaaS的笨办法
4. 🌍 独立开发出海，卡在收款
5. 📉 47个产品零收入，我复盘

> 建议首推标题 1 或 5（数字 + 反差感强，符合"真实记录"调性）。

## B.2 完整正文（约 360 字，双换行分段，每段 1–2 emoji 开头）

🤖 先说结论：一个人，靠 AI 编程，半年多攒了 47 个面向欧美的英文小 SaaS。

🌍 我不是技术大牛，也没团队。路子其实很笨：用 AI 编程工具批量生成 Next.js 小应用，丢到 Vercel 上线，接订阅收款，再用 AI 写内容去 X、IndieHackers 这些地方做 SEO 获客。

🛠️ 关键是"工厂化"。加一个新产品，基本就是改一份配置数据——生成、构建、部署、上架、写文案，全有脚本串起来。省下的时间都拿去想"卖给谁"。

📊 但这事真没那么光鲜。踩过的坑一只手数不过来：本地 Node 环境被改坏过，构建脚本指向了不存在的文件；沙箱把构建当成了批量删除直接拦掉；真实大模型接入时，环境变量因为脚本在错误目录运行，静默失败了三轮才查出来。

💡 我现在的打法是"批量出生，重点养 2–3"。47 个里挑出 AI 治理、隐私安全这一簇，三个产品定价 29 到 79 美元，主攻欧盟 AI 法案这个真实的时间点。

🌱 坦白讲，到现在还没拿到首笔真实收入。卡在一个必须本人办的收款资质环节，AI 替不了。但这套"低成本试错、用数据挑赢家"的循环，我觉得比单个产品成败更有价值。

🧭 如果你也在琢磨副业出海、AI 编程、或者一个人做点东西，评论区聊聊你卡在哪一关？主页还有几篇关于选利基和部署踩坑的碎碎念。

（发布时勾选 AI 辅助创作）

## B.3 封面文案建议（3:4 竖版 1080×1440，大字标题）

- **主标题**：我靠 AI 造了 47 个出海 SaaS
- **副标题**：一人公司 · 真实踩坑复盘
- **角标/标签条**：副业 / 出海 / AI 编程
- **备选钩子版**：一个人做 47 个产品，卡在哪一关？
- 设计提示：高对比底色 + 左对齐大字；避免精致摆拍感，用"草稿/截图"风更贴真实记录调性。

## B.4 发布时间建议

- 工作日：18:00–22:00（职场人通勤/睡前刷手机高峰）
- 周末：20:00–23:00
- 首推：周三或周四 20:00；或周六 21:00。首发后 2 小时内回评论区，借互动权重推流。

## B.5 合规自查清单（发布前逐条过）

1. **AI 标注**：发布页勾选「AI 辅助创作」；若发布前未做实质人工改写，正文开头补「本篇内容 AI 辅助生成」。
2. **无导流私域**：正文 / 评论 / 简介不含微信号、外链、二维码、谐音（薇 / 卫星 / vx）。
3. **无绝对化用词**：已排查 最 / 第一 / 顶级 / 唯一 / 100% / 完美 / 天花板 / yyds / 全网最佳 及其谐音变体（正文用"首笔"替代"第一笔"）。
4. **无虚假收益承诺**：未写"保赚 / 躺赚 / 被动收入"，如实写"暂无真实收入"。
5. **格式合规**：标题 ≤20 字、正文 300–1000 字、话题标签 3–8 个。
6. **引导走官方**：用「评论区聊聊 / 主页看更多」，不引导站外。

## B.6 话题标签（3–8 个，精准在前、泛话题在后）

#AI编程 #独立开发 #微SaaS #出海 #一人公司 #副业 #SaaS出海

---

*本文件为资产沉淀产出，与 `round-11-asset-strategy.md`（资产优先级规划）互补：那份定"先沉淀什么"，这份定"这条路怎么走 + 对外怎么讲"。*
