# IMA 知识库推送 · 状态与待办（2026-07-14）

> 主理人：易牧 ｜ 任务：② IMA 知识库推送（并行推进 R11 翻生产态）

## 一、连接器实测结论：只读，无法程序化推送

- 本会话 `ima-mcp ima知识库` 状态 = **connected**。
- 但 ToolSearch 仅暴露 **4 个读取工具**：
  `get_knowledge_base_list` / `get_knowledge_list` / `search_knowledge` / `fetch_media_content`。
  **没有任何上传 / 写入 / 导入接口。**
- 该结论与知识库内 `2026-07-13.md` 的工作日志诊断**完全一致**（那条日志本身就说"ima-mcp 连接器只有只读工具，无法从 WorkBuddy 把文件推到 ima"）。
- 因此：**从 WorkBuddy 自动把文件推送到 IMA 知识库，当前做不到。**

## 二、知识库现状（目标库 `一人公司出海项目` id `7482448057218648`）

- 已有 **44 条知识 / 625 KB**，内容以「小红书虚拟商品项目」为主（上传清单、营销笔记、分类总览、Notion 模板、AI 提示词包、部署报告、工作日志等），外加本项目的部分部署报告与导航文档。
- **OPC 出海项目 5 份核心文档尚未入库**（这正是跨机防重复劳动的关键缺口）：
  `products-catalog.md` / `skills-inventory.md` / `conventions.md` / `decisions-log.md` / `INDEX.md`（本地在 `IMA知识库/`）。

## 三、待上传文件清单（来自 KB 内「ima知识库上传清单(更新版)」）

共 **22 个**建议文件（知识型文档，路径均在 `E:\AgentCPM\07_一人公司出海项目\`）：

| 分组 | 文件 |
|---|---|
| A 总览/导航 | 项目索引.md、00_索引导航/产品代码索引.md、00_索引导航/流水线手册.md、00_索引导航/跨机迁移指南.md |
| B 部署/完成报告 | FINAL_DEPLOYMENT_REPORT_2026-07-07.md、DEPLOYMENT_COMPLETE_REPORT_2026-07-06.md、FINAL_COMPLETION_REPORT_2026-07-06.md、FINAL_AUTOMATION_REPORT_2026-07-05.md、FEATURE_VERIFICATION_FULL_REPORT.md、batch_deploy_report_20260707_2236.md、batch_deploy_report_20260707_1154.md |
| C 路线图/清单 | FEATURE_ROADMAP.md、MARKETING_CHECKLIST.md、PRODUCT_TEST_CHECKLIST.md |
| D 支付/接入 | STRIPE_PAYMENT_SETUP_GUIDE.md、WAFFO_QUICK_SETUP.md |
| E GEO/营销 | GEO_Content/LinkedIn_Articles_Templates.md、LinkedIn_Content_Calendar_30_Days.md、Product1-5/6-10/11-15/16-20_FAQ_and_Articles.md、sitemap_index.xml(可选) |

**另需补的 OPC 5 份**：`IMA知识库/INDEX.md`、`products-catalog.md`、`skills-inventory.md`、`conventions.md`、`decisions-log.md`。

## 四、正确推送路径（二选一，待用户拍板）

1. **手动上传（当前唯一可用）**：在你自己的 ima 客户端
   （PC 端 `ima.copilot` 或微信「ima 知识库」小程序，登录账号 `☆loVe☆╮Juan♡`）
   进入「知识库 → 一人公司出海项目」→ 点「上传」，按 A→E 分组把上述文件拖入，
   每组打标签（项目导航 / 部署报告 / 支付接入 / GEO内容）。
2. **git 同步兜底（待 github 重连）**：本会话 `github` 连接器 = disconnected，
   沙箱出网曾拦 github。重连后我把 `IMA知识库/` 5 份 + 必要文档推到 `workbuddy-skills`
   仓库，副机 `git pull` 即同步——这是两台电脑程序化共享的备选通道。

## 五、阻塞说明

- IMA MCP 只读 → 无法自动推送（已验证）。
- github 断开 → git 同步也暂不可用。
- **结论**：文件入 IMA 当前只能由你在 ima 客户端手动完成；我可在此继续准备
  清单/校对内容，但"上传"动作需你侧操作或等 github 重连。

## 六、与 R11 生产态任务的关系

R11 翻生产态（任务①）由后台代理独立执行，**不依赖 IMA 推送**，两者并行不冲突。
R11 上线后产生的生产 checkout 链接、真实收入验证结论，建议回填进
`decisions-log.md` / `products-catalog.md` 并随上述清单一并入 IMA，形成闭环。
