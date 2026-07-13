# 项目约定（Conventions）

> 所有产品、技能、文案必须遵守，保证一致性、可维护性、不踩历史坑。

## 定价数学（B2B 优先）
- $29/月 → 需 173 客达 $5k MRR；$49 → 102 客；$99 → 51 客。
- 消费端 $9/月 难成立（除非量大）。
- 当前实盘档位：B2B $19/$29/$39/$49/$59/$79/$99；R15 用 $15/$19（房产 Agent 客单价低）。

## 获客（不买量）
Build in Public（X + IndieHackers）＞ Product Hunt 首发 ＞ Reddit 细分版 ＞ GEO 目录（There's An AI For That / Futurepedia）＞ 开发者社区。

## 产品硬规则（防「功能≠宣传」历史 bug）
1. **恰好 2 档定价**：Free $0 + Pro。禁 Team / Studio / Enterprise。
2. `features` 数组**只列模板真实实现的能力**（LLM 文本渲染到页面）。禁声称 export / version history / API access / team workspace / priority support / collaboration。
3. 定价描述用 **"save history"**，绝不用 "export"。
4. **零 CJK**：所有生成文件英文（UI/code），slug ASCII。
5. 每个产品有真实有用的 `systemPrompt`，`/api/tool.ts` 返回有用输出。
6. slug **全局唯一**（上架前 grep 注册表 + 部署目录）。
7. 部署用 `--name <slug>`，使 Vercel 项目名 == slug。

## 真实 LLM 注入 SOP（关键）
- 每个产品 `/api/tool.ts` 在 serverless 运行时读 env（`OPENAI_API_KEY`/`OPENAI_BASE_URL`/`OPENAI_MODEL`）。
- Vercel 注入用 **REST API**（非 linked 目录，避免静默失败）：
  `POST https://api.vercel.com/v9/projects/{slug}/env`（`Authorization: Bearer $VERCEL_TOKEN`，`type:encrypted`，target 含 production）→ 再 `vercel deploy --prod` 重建生效。
- NVIDIA NIM：`nvapi-` key，端点 `https://integrate.api.nvidia.com/v1`，模型 `meta/llama-3.1-8b-instruct`。
- **验证**：`GET /v9/projects/{slug}/env` grep `OPENAI_API_KEY` 确认（非自报）。

## Waffo 上架 SOP
- 测试态：`WAFFO_USE_TEST=1` + TEST 私钥 → `create()` 铸造测试版本 → `publish()` → `checkout.anonymous.create` 取链接。
- 注册表防重防价格漂移；价格上限校验 $200/mo。
- 历史坑：用 PROD 私钥提前 create 的产品无测试版本，publish 报 "Target environment 'test' has no version" → 需 Dashboard 删除后重来。
- **⚠️ `create()` 非幂等（2026-07-13 实测，SDK 源码确认）**：`subscriptionProducts.create` 是裸 POST，**无幂等键、无 name 去重**。盲跑上架脚本会**重复创建 32 个产品并污染注册表**。→ 重跑前**必须**先查 `waffo_product_registry.json`：slug 已 `published` 且 PROD_ id 匹配 = 任务已完成，只需核验、不要重跑；只对确实缺失/失败的产品重跑。

## 合规护栏
- 英文文案无绝对词（best/perfect/guaranteed/#1/only/ultimate/100% 禁出现）。
- 法定数字仅作引用，不写结果承诺。
- R11 三款护栏句**必须保留**：AIActRadar「不替代律师」/ AgentRedTeam「不保证不被攻破」/ PrivScan「不发认证」。
- 小红书文案：AI 标注、零外链、避绝对词、标题≤20字/正文300-1000字/3-8标签。

## 工具链（沙箱注意）
- 系统 Node 24：`/c/Program Files/nvm/v24.12.0/npm.cmd`；全局 `vercel` 55。
- `next build` / `vercel deploy` / 任何 `api.vercel.com` 调用需 `dangerouslyDisableSandbox: true`（沙箱拦截网络）。
- `curl -o file` 在本沙箱失败（exit 23）→ 用 `curl ... > file` 重定向代替；`curl` 需 `-k`（SSL 握手）。
- 托管 Node 22 的 npm 已损坏，勿用。
