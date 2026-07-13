# Umami 自托管指南（R11 追踪层 · 大陆可达 + 欧美合规）

> 编写：易牧（主理人）｜2026-07-13｜用户已选定 Umami 自托管方案
> 目的：给你一台可达 VPS 跑 Umami，拿到两个值 `UMAMI_URL` + `UMAMI_ID`，交给代理注入 Vercel 环境变量。

## 为什么是 Umami
- **大陆可达**：自托管在你能访问的 VPS（CN 或新加坡节点），仪表盘对你零延迟、无 GFW 问题。
- **欧美合规**：无 Cookie、隐私友好，欧盟客户追踪不踩 GDPR。
- **事件驱动**：原生支持自定义事件，完美映射我们的 `generate_lead` / `begin_checkout` / `tool_used` / `purchase`。
- **免费 MIT**：docker 一键起，无订阅费。

## 一、1-click docker-compose（VPS 上）

```yaml
# docker-compose.yml —— 在 VPS 上新建目录运行
version: '3'
services:
  umami:
    image: ghcr.io/umami-software/umami:postgresql-latest
    ports:
      - "3000:3000"
    environment:
      DATABASE_TYPE: postgresql
      APP_SECRET: "<用 openssl rand -hex 32 生成一个>"
      DATABASE_URL: postgresql://umami:umami@db:5432/umami
    depends_on:
      db:
        condition: service_healthy
    restart: always
  db:
    image: postgres:15-alpine
    environment:
      POSTGRES_USER: umami
      POSTGRES_PASSWORD: umami
      POSTGRES_DB: umami
    volumes:
      - umami-db:/var/lib/postgresql/data
    restart: always
volumes:
  umami-db:
```

```bash
# VPS 上
mkdir -p ~/umami && cd ~/umami
# 把上面内容存为 docker-compose.yml，替换 APP_SECRET
docker compose up -d
```

## 二、拿两个关键值

1. 浏览器开 `http://<你的VPS IP>:3000` → 登录 `admin` / `umami`（**首次登录务必改密码**）。
2. 左侧 **Settings → Websites → Add website**：
   - Name：`R11 Winners`（或随意）
   - Domain：`aiactradar.vercel.app`（可填一个，后续三个 app 共用同一 Website ID 也行，或各建一个）
   - 保存。
3. 创建后在网站列表里点进去，**「Tracking Code」** 页面会显示：
   - `data-website-id="<这是 UMAMI_ID，一串 UUID>"` ← 复制
   - 脚本地址 `<UMAMI_URL>/script.js` ← `UMAMI_URL` 即你的实例地址

> ⚠️ **必须 HTTPS**：R11 三款 app 都是 HTTPS 站点，Umami 脚本地址也必须是 `https://`，否则浏览器因 mixed-content 拦截。**生产务必给 Umami 套反代 + Let's Encrypt（Caddy 最简单）**。快速测试可用 `http://IP:3000` 但仅能验证、正式跑要 HTTPS。

## 三、把值交给我（或自己设 Vercel 环境变量）

拿到后告诉我：
- `UMAMI_URL`（如 `https://umami.你的域名.com`）
- `UMAMI_ID`（UUID）

我/代理会写入 3 款 R11 app 的 Vercel 环境变量（同 LLM 密钥的 REST SOP）：
- `NEXT_PUBLIC_UMAMI_URL`
- `NEXT_PUBLIC_UMAMI_ID`
- `WAFFO_WEBHOOK_SECRET`（Waffo 后台 **Webhook 签名密钥**，HMAC 共享密钥，用于验签 purchase 回调；不是 RSA 公钥）

设完需 **redeploy** 让 `NEXT_PUBLIC_*` 编译进前端。

## 四、Waffo Webhook 登记（用户后台操作）
在 Waffo 商户后台（pancake.waffo.ai）为 R11 三款所在 store 登记 Webhook：
- URL：`https://<app>.vercel.app/api/waffo-webhook`（三款各一个，或共用一个带 slug 区分）
- 订阅事件（Pancake 事件集）：
  - `order.completed`（一次性付款）
  - `subscription.activated`（订阅首付款）
  - `subscription.payment_succeeded`（续费）
  - `refund.succeeded` / `refund.failed`（仅记录，不计入 Umami 收入）
- 复制 **Webhook 签名密钥 (secret)** 给我（写入 `WAFFO_WEBHOOK_SECRET`，这是 HMAC 共享密钥，不是 RSA 公钥）

> 代理已写好 `pages/api/waffo-webhook.ts`：验签 → 调 Umami `/api/send` 发 `purchase` 事件（带 value/transaction_id/items）。这是收入事件真正闭环的唯一通道。

## 五、验证
1. Umami 仪表盘 Realtime 看到 `generate_lead` / `begin_checkout` 流入 = 前端追踪 OK。
2. 用 Waffo 测试卡走一笔 → webhook 触发 → Umami 出现 `purchase` 事件 + 金额 = 收入闭环 OK。
3. 翻生产态真实扣款后，真实收入即显示在 Umami。
