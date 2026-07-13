# GA4 接入与 R11 真实收入闭环 · 打通清单

> 编写：易牧（主理人）｜2026-07-13｜基于磁盘/源码/REST 实测，非代理自报

## 〇、当前真实状态（已独立核实）

| 项 | 状态 | 证据 |
|---|---|---|
| R11 三款部署 | ✅ READY（生产 URL 在线） | Vercel REST 实测 `readyState=READY`，created 2026-07-13 |
| GA4 Measurement ID | ❌ 占位符 `G-REPLACE_ME` | 三款 `lib/analytics.ts` 第 6 行，grep 确认 |
| 客户端事件 generate_lead / begin_checkout / tool_used | ✅ 代码已接（CTA 点击触发） | `lib/analytics.ts` + `pages/_app.tsx` |
| 收入事件 purchase | ❌ 注释占位，未实现 | `lib/analytics.ts` 51–59 行被注释，设计由服务端 webhook 触发 |
| Waffo 产品模式 | ❌ 全量 76 款仍为测试态 | 注册表 `environment: test`，测试卡不真扣款 |

**结论：收入显示为零是"链路未闭合"，不是配置 bug。** 三个断点：① 测试态无真订单；② GA4 ID 占位无上报；③ purchase 事件无服务端触发。

---

## 一、GA4 Measurement ID 获取（控制台步骤）

前置：需已有一个 GA4 媒体资源 + 网站数据流。没有则先建。

1. 打开 https://analytics.google.com → 左下角 **⚙ 管理（Admin）**。
2. 在 **「媒体资源（Property）」** 列，点 **「数据流（Data Streams）」**。
3. 选已有的 **「Web」** 数据流（如没有 → 点「添加数据流」→ Web → 填域名 `aiactradar.vercel.app`、流名称 → 创建）。
4. 进入该数据流详情页，**顶部「衡量 ID（Measurement ID）」** 即 `G-XXXXXXX`（格式固定 G- 开头 + 10 位字母数字）。
5. 把这一串复制给我（三款 app 用**同一个** ID 即可，也可各建一个流分别看）。

> 注意：GA4 后台「实时（Realtime）」和「DebugView」可用于验证事件是否流入；「变现 → 电商采购（Monetization → Ecommerce purchases）」才是看收入的地方。

---

## 二、注入 ID + 重建 + 部署（完整流程，每款 app）

> 仅改 `lib/analytics.ts` 第 6 行（GA4 ID），其余不动。改完用系统 Node 24 工具链构建部署（托管 Node 22 的 npm 已损坏）。

**手动/脚本替换（以 aiactradar 为例）：**
```bash
# 1) 把占位符换成真实 ID（三款都改同一文件 lib/analytics.ts 第6行）
#    export const GA4_MEASUREMENT_ID = 'G-XXXXXXX'

# 2) 切系统 Node 24 工具链（关键）
export PATH='/c/Program Files/nvm/v24.12.0:$PATH'

# 3) 构建+部署（沙箱需放行网络/写盘）
cd "E:/AgentCPM/07_一人公司出海项目/12_Micro_SaaS出海/<slug>"
tar --exclude=node_modules --exclude=.next --exclude=.vercel -cf - . | (mkdir -p /tmp/<slug> && tar -xf - -C /tmp/<slug>)
cd /tmp/<slug>
npm.cmd install
npm.cmd run build
vercel deploy --prod --yes --name <slug> --token $VERCEL_TOKEN
```
对 `aiactradar` / `agentredteam` / `privacyscan` 各跑一遍。

**给我 ID 后，我可直接代执行**（后台代理跑，磁盘报告为真相源，避免本会话中断丢进度）。

---

## 三、IMA 多设备登录（已查官方资料）

腾讯 IMA 知识库结论（来源：ima.qq.com 官方 wiki + 腾讯云开发者文档）：

- **登录方式**：仅微信扫码登录，无密码/手机号登录。
- **多设备同时登录**：✅ **同一微信账号可在多端（Windows/Mac/手机/小程序/网页/鸿蒙等九端）同时登录，实时云端同步，无设备数量限制、无登录冲突。** 你担心的"多设备同时登录限制"不存在。
- **团队共享库**：需成员用**统一企业微信**加入，可设「仅查看 / 可编辑」权限。
- **隐私**：个人私密库仅本人可见；共享库按权限控制。

**两机协作的实操注意（比登录限制更关键的是编辑冲突）：**
- IMA 实时同步是"最后写入胜出"，**两台电脑同时编辑同一篇文档会互相覆盖**。建议：
  - **git 仓库（`workbuddy-skills`）作唯一真相源**，IMA 作「导入 / 只读查阅」目标，不要两边都在 IMA 里改同一篇；
  - 按文件分工（如本机管 products-catalog / conventions，副机管 decisions-log / skills-inventory），避免撞车。
- **本会话 IMA 连接器仍是 disconnected**，我无法直接推送。两种落地方式：
  - 你在本机或副机打开 IMA → 新建/选共享库 → 把 `IMA知识库/` 下 5 份 md（INDEX / products-catalog / skills-inventory / conventions / decisions-log）**手动导入**；
  - 或我先走 git 同步（之前 `workbuddy-skills` 仓库就是给副机 pull 用的），沙箱出网曾拦 github，需放行。

---

## 四、打通「真实收入数据」的必要动作（按顺序）

1. **【你提供】GA4 ID** → 我注入 3 款 + 部署 → `generate_lead` / `begin_checkout` / `tool_used` 开始有数（但仍非收入）。
2. **【你决策】R11 三款翻 Waffo 生产态**（KYB 已批准，技术上可翻）→ 产生**真实扣款订单**。建议先只翻这 3 款验证闭环，再扩全量。
3. **【需新建】服务端 purchase webhook**：Waffo 的 checkout / subscription 成功回调 → 调 GA4 Measurement Protocol 发 `purchase` 事件（带 `value`/`transaction_id`）。**这一步不做，GA4 里永远看不到收入。** 这是当前最大缺口。
4. **验证**：GA4 后台 Realtime 看 `begin_checkout` 流入；Monetization → Ecommerce purchases 看收入；DebugView 排错。

> 优先级建议：先给我 GA4 ID 做 1（半天内可上），同时拍板 2（翻生产态），webhook（3）我可并行设计。三者齐了才有真实收入数据可"验证"。

## 五、本次核实补充（2026-07-13 21:5x）

- **项目内无任何真实 GA4 ID**：全项目 grep（`G-`/`GTM-`/`UA-`/`MEASUREMENT_ID`/`gtag`）仅命中 `G-REPLACE_ME`（3 款 R11 app + `_gtagbuild` 临时目录）与 `G-xxxxxxxxxx` 模板（`payment_config/env_template.md`）；`_keys.env` 无 GA 密钥；`08_代码库/meeting-mind/.env` 也无。→ 不存在"捡现成 ID"。
- **GA 控制台访问限制**：用户无法直接访问 analytics.google.com（疑似网络封锁，大陆常见无 VPN 不可达）；本沙箱同样无 Google 会话且连不上 Google 系服务 → **浏览器自动化无法抓取私有 ID**。获取 ID 只能靠：① 用户/副机已有属性则粘贴 `G-XXXXXXX` 或截图；② 尚无属性则需在有 VPN/海外的副机创建；③ 或改接国内可达分析（Plausible/Umami/百度统计）做创始人看板、GA4 留作标准管道。
- **推荐接法（与现有模板一致）**：`payment_config/env_template.md` 已定义 `NEXT_PUBLIC_GA_ID`，故应将 `lib/analytics.ts` 改为读 `process.env.NEXT_PUBLIC_GA_ID`，ID 经 **Vercel 环境变量**注入（同 LLM 密钥 REST SOP），不再硬编码 `G-REPLACE_ME`。代码无密钥、可随时轮换无需改码重发。
- **收入盲区未变**：`purchase` 事件仍注释占位，需服务端 webhook（Waffo 成功回调 → GA4 Measurement Protocol）才能上报收入；该 webhook 与 GA ID 注入可并行推进（均运行时读 env）。

---

## 六、大陆可达性核查（联网查证 2026-07-13）+ 替代方案建议

**用户原问：给的链接在大陆是不是无法访问？** 已查证，分两个链接：

### 6.1 analytics.google.com —— 确认大陆无法直连
多家来源一致结论（appinchina.co / 10100.com / graphed.com 等）：
- GFW 屏蔽 `google-analytics.com` 与 `googletagmanager.com`，**GA4 控制台不挂 VPN 进不去**；
- 即便页面塞了 gtag，**大陆访客脚本请求超时（最长卡 3 分钟）**拖垮性能，且 CN 访客数据发不到 Google → 报表残缺；
- 合规风险：境内访客行为数据出境违反《个人信息保护法(PIPL)》。
- 结论：**GA4 既建不了属性（需 Google 登录），也看不了后台（CN 无 VPN 不可达）**，对"大陆运营者自己看数据"是死路。

### 6.2 ft07.com（OPC 方法论原书站）—— 大陆可达
- 方糖07 国内站，面向中文独立开发者，搜索引擎大量 CSDN/Gitee 镜像，**无封锁迹象**，可正常打开。OPC 模板里的配套阅读链接无碍。

### 6.3 关键认知：客户在欧美，GA4 能追踪；卡的是"运营者自己看"
- 欧美访客处无 GFW，GA4 对彼处客户能正常采集。
- 但**创始人位于 CN**：建属性需 Google 登录（不可达）、看后台需 VPN。→ GA4 不宜作创始人主看板。

### 6.4 国内可达 + 欧美合规的替代（研究对比）

| 工具 | 大陆可达 | 欧美 GDPR | 事件追踪 | 成本 | 推荐 |
|---|---|---|---|---|---|
| **Umami（自托管）** | ✅ 自托管在可达 VPS | ✅ 无 Cookie 隐私友好 | ✅ 自定义事件 | 免费 MIT | ⭐⭐⭐⭐⭐ |
| Plausible（自托管） | ✅ | ✅ | ✅ | 免费/云版付费 | ⭐⭐⭐⭐ |
| 百度统计 | ✅ | ❌ 隐私采集重 EU 风险 | 部分 | 免费 | ⭐⭐ 仅 CN 兜底 |
| GA4（副机建+VPN看） | ❌ 需 VPN | ✅ | ✅ | 免费 | 不推荐主用 |

### 6.5 建议方案：追踪层从 GA4 换成 **Umami 自托管**
- 一个工具同时解决：① 创始人在 CN 能看后台；② 欧美客户合规追踪（无 Cookie、GDPR 友好）；③ 事件驱动（映射 generate_lead/begin_checkout/purchase）。
- 接法与现有模板一致：读 `process.env.NEXT_PUBLIC_UMAMI_ID` + `NEXT_PUBLIC_UMAMI_URL`，经 **Vercel 环境变量**注入（同 LLM 密钥 REST SOP），不硬编码。
- 自托管：给一台可达 VPS（CN 或新加坡节点）跑 `umami` docker 镜像即可；若 VPS 在 CN 则仪表盘对创始人零延迟。我可给 1-click `docker-compose.yml`。
- **收入盲区不变**：无论 Umami 还是 GA4，`purchase` 仍需**服务端 webhook** 触发（Umami 用其 HTTP API 发事件；GA4 用 Measurement Protocol）。该 webhook 与追踪层切换可并行推进。

> 待用户拍板：选 Umami（推荐）/ GA4 副机建 / 百度统计 / Plausible 其一，我再改 `lib/analytics.ts` + 部署 + 写 purchase webhook。
