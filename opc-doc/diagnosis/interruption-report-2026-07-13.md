# 任务中断诊断与修复报告

- 诊断时间：2026-07-13 08:46 (GMT+8)
- 诊断人：OPC 主理人 易牧（自动核实）
- 触发：用户多次询问"前面任务是否完成 / 是否因上下文过长中断"

---

## 一、中断现象（用户侧感知）

1. 自主开发会话中，主理人先后启动两条后台流水线：
   - R13 部署 + 注入真实 LLM（`YNNMKZ`）
   - R12 三款修正产品重部署（`zKgMcP`）
2. 后台任务启动后，用户**连续多次**追问"任务完成没有 / 是否提前中断 / 是不是上下文太长"。
3. 对话顶部出现了 `<cb_summary>` 标记，说明**上一轮长对话被平台自动压缩（compaction）并重建了上下文**。

---

## 二、硬证据收集（不靠记忆，实测为准）

| 证据 | 结果 | 含义 |
|---|---|---|
| `r13_deploy_report.txt`（07:09） | 8 款全 `DEPLOY_OK`+`ENV_OK`，末行 `R13_DONE` | R13 脚本自报成功 |
| R13 八款源码目录 | 全部 `SRC_OK` | 生成未丢失 |
| Vercel REST API 实测（`r13_reaudit_0844.json`） | R13 八款 + R12 三款修正产品 `OPENAI_API_KEY=1` | **真实 LLM 已注入生效** |
| `verify_deploy_state.sh` 全量核验（`deploy_state_0846.json`） | **60/60 产品 `real_llm=1`** | 全部在售产品真实 LLM 就绪 |
| 对话 `<cb_summary>` 标记 | 存在 | 会话曾被压缩重建 |

**结论：执行层没有失败。** R13 部署、LLM 注入、R12 修正、36 款 mock 修复全部真实完成。Vercel 侧 60/60 真实 LLM 是客观事实。

---

## 三、根因诊断

### 直接原因：上下文窗口耗尽 → 会话自动压缩 → 后台任务回执链路被切断

不是以下任何一项：
- ❌ 网络超时（Vercel API 与部署均成功，curl 实测连通）
- ❌ API 调用错误（无 429/5xx，REST 注入返回成功）
- ❌ 资源不足（Vercel 构建未触发 45 分钟超时、无 OOM/SIGKILL）
- ❌ 代码/部署失败（R13 报告 `DEPLOY_OK`、Vercel 实测 `key=1`）

而是：
- ✅ **上下文窗口耗尽**：长自主会话累积了大量工具调用、文件读取、后台任务输出，逼近上下文上限。
- ✅ **平台触发自动压缩（compaction）**：生成 `cb_summary`，旧上下文结束、新上下文以摘要重启。
- ✅ **后台任务完成回执丢失**：R13/R12 后台任务在压缩前/中完成，其完成通知本应"唤醒"等待中的 agent loop，但压缩后 loop 已被替换，通知无法干净送达。
- ✅ **结果**：用户侧看不到"完成"信号，于是反复追问；重召唤的 agent 也只能靠"重新实测 Vercel"来确认状态——这正是正确恢复路径。

### 机理对齐（联网资料印证）

- **上下文窗口非无限，长任务必耗尽**（walkinglabs《让跨会话任务保持上下文连续》）：agent 不只生成代码，还要理解代码库、跟踪决策、处理工具输出，信息增长快于窗口扩容。
- **单一 Agent 的 context 膨胀链条**（sotasync）：~80k tokens 逼近 compact 阈值 → 触发 compact → 遗忘细节 → 重复检查/退出。本会话正是此链条。
- **四种中断场景**（claudelab）：①上下文耗尽 ②限流 ③工具执行失败 ④网络断开。本例属①，典型症状是"continuing… 后突然停止 / 回复被截断"。
- **中断≠失败**（knightli）："An interrupted AI Agent task is not automatically a failed task. It becomes a failed task only when there is no external state to recover from." 本会话有 `r13_deploy_report.txt`、Vercel 环境变量等外部状态，故可无损恢复。
- **context rot / lost-in-the-middle**（redis）：窗口越大质量越退化，重要细节若落在中部会被忽略；解法=把持久状态移出窗口、存入外部存储。

---

## 四、解决方案

### A. 已执行的恢复（验证即恢复）

1. **以外部状态为真相源**：不信任聊天记忆，直接 `curl` Vercel REST API 核验 `OPENAI_API_KEY` 注入状态 → 确认 60/60 真实 LLM 已就绪。
2. **无需重跑任何部署**：执行层本已完成，重跑只会重复劳动、制造风险。

### B. 预防措施（针对本项目落地）

**B1. 建立可复用的外部状态核验脚本（已落地）**
- `product-factory/verify_deploy_state.sh`：从 `waffo_product_registry.json` + R12/R13 已知 slug 提取全量产品，逐一 `GET /v9/projects/{slug}/env` 核验 `OPENAI_API_KEY`，输出 `deploy_state_<HHMM>.json` + 摘要。
- **用法**：任何自主会话启动/恢复时，先跑它而不是读聊天记忆。
- 刚运行结果：`60/60 产品已注入真实 LLM`。

**B2. 后台任务自带完成标记（已具备，保留并强化）**
- 每个部署脚本末尾写 `r{N}_deploy_report.txt`（`DEPLOY_OK`/`ENV_OK`/`R{N}_DONE`）。恢复时**先读报告文件**，再决定是否补跑。

**B3. 压缩/重建后"先核验再继续"协议**
- 新会话第一动作：跑 `verify_deploy_state.sh` + 读最近的 `r{N}_deploy_report.txt` → 重建最小上下文 → 只续做 pending 项。
- 禁止"凭记忆说继续"；禁止"无核验就重跑全量"。

**B4. 控制上下文体积（redis 六步法摘录）**
- 不在聊天内联大文件内容，只引用路径。
- 工具输出设上限/分页；后台任务输出不回灌主上下文。
- 压缩后丢弃旧失败尝试、冗余讨论，只保留：当前目标、已改文件、已做决策、pending、验证结果。

**B5. 任务分批 + 显式停报点（sotasync 主-子 Agent 模式）**
- 长自主循环按"可验证小批次"推进（如每轮一个产品簇 Rn），每批结束显式汇报并落盘，而非无边界长跑。
- 受 `maxTurns`（本 expert=200）约束，分批可避免单轮触顶后被截断在中间态。

**B6. Vercel 构建健壮性（针对未来部署）**
- `vercel deploy` 远程构建，硬上限 45 分钟、Standard 容器 8GB 内存。当前 Next.js 产品极简，风险低。
- 若构建卡在 "Building" 不退出：参考 Vercel KB，在构建钩子加 `setTimeout(() => process.exit(0), 0)` 强制退出。
- 仅改环境变量时可用 `vercel deploy --prebuilt`（本地 `vercel build` 后上传）跳过重复构建，提速且降低超时概率。

---

## 五、当前任务进度看板

| 任务 | 状态 | 进度 | 备注 |
|---|---|---|---|
| Week-1 发布内容包 + GA4 | ✅ 已完成 | 100% | `r11-week1-content-ga4.md` / `r11-week1-launch-copy.md` |
| 小红书种草文案 + 合规 | ✅ 已完成 | 100% | `r11-journey-summary-xhs.md`，已联网核 2026 规范 |
| 36 款 mock 修复（真实 LLM） | ✅ 已完成 | 100% | 实测 60/60 全就绪 |
| R12 开发者工具簇部署 | ✅ 已完成 | 100% | 8 款真实 LLM |
| R13 电商/店铺运营簇部署 | ✅ 已完成 | 100% | 8 款真实 LLM，报告 `R13_DONE` |
| 过度承诺修正 | ✅ 已完成 | 100% | 删 2 处虚构功能 + R12 三款 desc 措辞 |
| 全量静态审计 | ✅ 已完成 | 100% | `audit_features.md` |
| 代理优先级排序 | ✅ 已完成 | 100% | `product_priority_proxy.md`（启发式，无真实数据） |
| 可选收尾A：资产登记 | ✅ 已完成 | 100% | `asset-inventory.md` |
| 可选收尾B：R12 模板 | ✅ 已完成 | 100% | `r12-launch-template.md` |
| **凝练开发流程为 Skills + 优化已有 Skills** | ⏸ 待开始 | 0% | 用户曾要求，会话中断前未执行 |
| **R14 小簇（视时间）** | ⏸ 未启动 | 0% | 原定"距早8点约1h再开"，未触发 |
| **18 孤儿目录清理** | ⏸ 建议未做 | 0% | 不在售，无隐患，建议后续删 |
| **Waffo KYB（用户侧）** | 🚧 阻塞 | — | 用户亲办，真实收款前置 |

---

## 六、后续跟进建议

1. **立即可做（无需你操作）**：本轮已生成 `verify_deploy_state.sh` 与外部快照，后续任何会话恢复先跑它。
2. **待你拍板**：是否执行"凝练开发流程为 Skills"——这是你此前明确提出、因中断未落地的项，建议作为下一轮首项。
3. **你侧唯一阻塞**：提交 Waffo KYB（Dashboard → Business Details + Payout Account）。KYB 过 + 把 21 篇文案 `[LIVE_SOON]` 替换为真实结账链接即一键点火。
4. **防患**：后续长自主会话一律"分批 + 每批落盘报告 + 恢复先核验"，避免再次因压缩丢失回执。

---

## 七、参考来源（联网检索）

- walkinglabs《让跨会话的任务保持上下文连续》— 上下文窗口有限、状态持久化
- sotasync《Claude Code 跑一半就停：单一 Agent 的 context 瓶颈与主-子架构解法》
- claudelab《Claude Code Stopped Mid-Task? A Practical Recovery Guide》
- knightli《How to Resume an Interrupted Long-Running AI Agent Task》
- redis《Your agent hit the context limit. Here's the playbook》
- Vercel KB《Fixing deployments that hang after the build step succeeds》
- Vercel KB《Build step did not complete within the maximum of 45 minutes》
- Vercel KB《How can I set a custom build timeout?》
