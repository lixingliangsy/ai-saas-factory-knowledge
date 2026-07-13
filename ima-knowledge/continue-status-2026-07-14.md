# 继续推进状态 — 2026-07-14（主理人独立核验）

> 本文件为 OPC 主理人对「R11 翻生产态」+「IMA 推送」两项并行任务的继续推进核验结论。

## 任务① R11 三款翻 Waffo 生产态

### 独立核验结论（curl 实测 + 注册表 on disk，非采信自报）
- 三款生产扣款链接实测 **HTTP 200**（直连 pancake.waffo.ai，curl -k）：
  - privacyscan → `cs_e7f1f067-dfcd-ed53-6177-f3ef05144c11` ✅ 200
  - aiactradar  → `cs_9be878dc-bafe-683a-239d-b389562c903d` ✅ 200
  - agentredteam→ `cs_dd293263-537c-dd22-78ab-072adcc1624f` ✅ 200
- 注册表确认三款均带 `prodPublished:true` / `prodCheckoutUrl` / `prodLaunchedAt`，顶层 `r11WinnersProduction` 块存在。
- 注册表摘要未变：**total 79 / published 76 / failed 3**。

### 当前状态
- 三款已处于 **PRODUCTION 真实扣款态**，KYB 已批。这就是用户要求的"小范围验证真实收入"canary 范围（仅 R11 三款）。
- ⚠️ **Umami 收入追踪未激活**：缺 3 个环境变量 `NEXT_PUBLIC_UMAMI_URL` / `NEXT_PUBLIC_UMAMI_ID` / `WAFFO_WEBHOOK_SECRET`。未设前真实购买不会被追踪（webhook 走跳过验签转发兜底，但 Umami 无数据）。

### 待用户确认/提供
1. 三个 Umami/webhook 变量值 → 注入 Vercel + 重部署 → 真实 purchase 事件进 Umami。
2. canary 验证方式：是否用测试卡 `4576 7500 0000 0110` 自测一次真实扣款 + webhook 回传？还是等自然流量？
3. 验证正常后是否扩大范围（76 published 全翻 live 或挑高潜子集）？

### 仍 failed 待用户处理
- `vertical-crm` / `policyforge` / `schemasafe`：早期用 PROD key 创建、无 test version，publish 失败。需 Waffo Dashboard 删除后用 TEST key 重建（不在 76 published 范围）。

## 任务② IMA 知识库推送

### 连接器实测结论（本轮再次确认）
- `ima-mcp` 仅暴露 4 个只读工具：get_knowledge_base_list / get_knowledge_list / search_knowledge / fetch_media_content。
- **无任何 upload/import/write 接口 → 程序化推送不可行**。
- 知识库「一人公司出海项目」(id `7482448057218648`) 现有 44 条，多为小红书虚拟商品项目；**OPC 出海 5 份核心文档（products-catalog / skills-inventory / conventions / decisions-log / INDEX）未入库**。

### 剩余推送路径（需用户手动操作）
- 路径 A（当前唯一可行）：在 ima 客户端手动上传待传清单（22 文件 + OPC 5 份），清单见 `opc-doc/outputs/08-asset/ima-push-status.md`。
- 路径 B（兜底）：等 github 重连后走 git 同步（当前 github disconnected）。

### 待用户操作
- 在 ima 网页/客户端打开「一人公司出海项目」知识库，按 `ima-push-status.md` 清单手动上传；
- 或确认是否存在 ima 开放 API token 可供程序化写入。

## 阻塞汇总
| 阻塞点 | 责任方 | 状态 |
|---|---|---|
| Umami 3 变量缺失 | 用户 | 待提供 |
| R11 真实购买验证 | 用户决策 | 待确认 canary 方式 |
| IMA 程序化推送不可行 | 连接器限制 | 转手动上传 |
| 3 个 failed 产品 | 用户(Dashboard) | 待删除重建 |

## 续跑补充 — 自主运行窗口（07-14 00:30「全自主至 08:00」指令）

### 已落盘产出（后台代理，磁盘证据）
- **小红书种草**：`marketing_content/xhs_ai_coding_overseas.md`（23KB，10 篇笔记 + 配图提示词，仅本地未发布）。
- **LinkedIn / Indie Hackers 文案**：`marketing_content/linkedin_ih_drafts.md`（20KB，英文，真实数据无编造）。

### GitHub 同步 — 主理人侧核查结论
- 后台代理仅产出**本地预备清单**（`automation_logs/_manifests/*.json` + `_measure.py`）：逐产品扫描源码、剔除密钥后打包的待推送 JSON，**非已推送证据**。
- 主理人环境**无任何 GitHub 凭证**（`gh` 未登录 / 无 token / 无 gh 配置）→ 无法自行推送或核验远端。
- 真实推送仅能由后台代理经其 GitHub MCP 完成；lead 侧待其完成通知方可知结果。不重复派代理以免冲突。

### Canary 自测 — webhook HMAC 验证层（9/9 通过）
- 脚本 `automation_logs/canary_webhook_test.mjs`（逐字复刻 `aiactradar/pages/api/waffo-webhook.ts::hmacVerify`）。
- 覆盖：有效 hex/base64 签名、`v1=`/`sha256=`/`t=,v1=` 前缀剥离、错签名/缺密钥/错密钥/空头 拒绝 → 全 PASS。
- 结论：验证层逻辑正确；端到端闭环仍阻塞于用户提供的 `WAFFO_WEBHOOK_SECRET` + 真实 Umami(`NEXT_PUBLIC_UMAMI_URL`/`ID`) 三值（不可自动生成）。

### 当前阻塞（窗口内无法自主消解）
1. Umami/Webhook 三值缺失 → 待用户提供后批量注入 Vercel + 重部署。
2. IMA 连接器只读 → 转用户手动上传（见 `ima-push-status.md`）。
3. 3 个 failed 产品 → 待 Dashboard 删除后用 TEST key 重建。
4. R11 扩全量 76 款 → 待用户决策。

## GitHub 同步最终结论（代理回执汇总，截至 m-r/s-z 回执）

### 根因（比「缺建仓权限」更硬）
GitHub MCP 集成以 `lixingliangsy` 身份鉴权成功，但安装令牌**仅有 `Contents: read`**（public_repos=15）。所有写操作均 403：
- `create_repository` → 403
- `push_files` → 403（POST .../git/trees）
- `create_or_update_file` → 403
读操作之所以能工作，仅因那 15 个旧仓库是**公开可读**，不等于有写权限。
**结论：即便是已存在的仓库也无法被推送**（promptgen-pro / snippetvault / chartmind-ai 实测 push 同样 403）。但需注意：现有 15 个公开仓库中**确有与当前产品 slug 重合者**——a–f 段 7 个（calorie-tracker、ai-fitness-plan-generator、ai-fitness-planner、chartmind-ai、fitness-challenge-tracker、fitness-community-platform、fitness-gamification-platform）、m–r 段 2 个（promptgen-pro、running-training-plan）、s–z 段 ≥1 个（snippetvault）等。写权限恢复后，这些重合仓库可直接 `push_files` 覆盖同步；其余不重合的需 `create_repository` 新建后再推送。无任何本地绕过（gh 未登录、无 PAT、MCP 代理需会话鉴权）。

### 各段计数（代理回执）
| 段 | 负责代理 | slug 数 | 已建仓 | 已推送 | 403 阻塞 |
|---|---|---|---|---|---|
| docs(知识库) | gp-7 | 9 | 0 | 0 | 9 |
| g–l | gp-4 | 18 | 0 | 0 | 18 |
| m–r | gp-5 | 26 | 0 | 0 | 26 |
| s–z | gp-6 | 26 | 0 | 0 | 26 |
| a–f | gp-3 | 42 | 0 | 0 | 42 |
| **合计** | | **121** | **0** | **0** | **121** |

> 注：121 = 知识库 9 + 产品 112（a–f 42 / g–l 18 / m–r 26 / s–z 26）。其中约 15 个产品仓库在 GitHub 已存在（写权限恢复后可覆盖同步），其余需新建。

### 已就绪（不丢失）
- 各段过滤后的源码清单已落盘：`automation_logs/_manifests/*.json`（m-r/s-z 段）、`automation_logs/staging-gl/_manifest_<slug>.json`（g-l 段），均为 `{path, content}` 就绪 payload。
- 过滤规则：排除 node_modules/.next/.vercel/.git/.buildtmp、`.env*`/secrets.env、package-lock.json、>20KB 二进制图、`out/`（Next 静态导出产物）；密钥扫描剔除（如 pricelens 已省 5 个含支付/Waffo 密钥文件）。零源码改动。

### 唯一解阻塞路径（用户操作其一）
1. 给 GitHub App/集成授予 **`Contents: write` + Repository creation**（或 fine-grained token 带 `public_repo`/`repo`）；或
2. 提供带 `repo` scope 的 PAT；或
3. 手动在 github.com/new 建好仓库（public, main 分支, 带 README）后走覆盖模式。
权限到位后，所有 staging payload 可立即 `push_files`，无需重做收集。
