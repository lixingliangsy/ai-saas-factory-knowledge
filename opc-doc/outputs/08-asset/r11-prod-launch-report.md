# R11 Winner Products — Production (Real-Money) Launch Report

**Generated:** 2026-07-13T16:19:32Z (local 2026-07-14)
**Operator:** integration worker (general-purpose-1)
**Store:** `pancake.waffo.ai` — merchant `lixingliang` (`MER_1vgmBs6nKCAQoIWDtJDmaG`), store `STO_6p1cnwJDlk6gzVNUCQdWqN`
**KYB status:** APPROVED (precondition for real-money publishing — confirmed before work began)
**SDK:** `@waffo/pancake-ts` v0.11.0 (CommonJS build at `12_Micro_SaaS出海/pricelens/node_modules/@waffo/pancake-ts`)

---

## ⚠️ REAL-MONEY WARNING

The three checkout links below are **LIVE PRODUCTION links**. Any purchase completed through them
**charges real money** to the buyer and settles to the approved Waffo merchant account.

- **No purchase was executed during this launch.** Verification was limited to confirming each
  checkout page returns **HTTP 200** (GET/HEAD only).
- Do **not** click-through and pay on these links unless that is an intentional, authorized test purchase.
- Umami revenue tracking is **NOT yet active** — see the env-var warning at the bottom of this report.

---

## Canary outcome (privacyscan — done FIRST)

Procedure required the canary (`privacyscan`) to be fully verified before touching the other two.

1. `subscriptionProducts.publish({ id: PROD_138AfPOZzeXZlwip728Vzu })` was called with the **PROD key**.
   The API returned `400 Already published to production` — i.e. the product was **already live in
   production** (published earlier with a production API key). This is a success-equivalent for our
   purposes, not an auth/environment error, so the TEST-key retry was correctly **not** triggered.
2. A fresh production checkout link was minted via `checkout.anonymous.create({ productId, currency: 'USD' })` → `cs_e7f1f067-…`.
3. `curl -sk -o /dev/null -w "%{http_code}"` returned **200** (both GET and HEAD).

**Canary = CONFIRMED GOOD.** Expanded to `aiactradar` + `agentredteam` (same steps).

> Note on "already published": all three R11 winners were already in production mode at launch time
> (the PROD-key publish call reported `Already published to production` for each). The launch
> therefore (a) confirmed prod status and (b) minted current real-money checkout links. The
> `publishKeyUsed` recorded below is **PROD** for all three, consistent with the API's guidance to
> "use update() with a production API key" for the live product.

---

## Per-product results

| # | Slug | PROD_ id | Publish key used | Publish status | Prod checkout URL | Link HTTP |
|---|------|----------|------------------|----------------|-------------------|-----------|
| 1 (CANARY) | privacyscan | `PROD_138AfPOZzeXZlwip728Vzu` | **PROD** | active (already in prod) | https://pancake.waffo.ai/store/lixingliang-ai-tools-6cilbw8v/checkout/cs_e7f1f067-dfcd-ed53-6177-f3ef05144c11 | **200** |
| 2 | aiactradar | `PROD_0sib87Bs2jaNx3mxg8NOJM` | **PROD** | active (already in prod) | https://pancake.waffo.ai/store/lixingliang-ai-tools-6cilbw8v/checkout/cs_9be878dc-bafe-683a-239d-b389562c903d | **200** |
| 3 | agentredteam | `PROD_3bT77yn32ExkyXzPZZLjmK` | **PROD** | active (already in prod) | https://pancake.waffo.ai/store/lixingliang-ai-tools-6cilbw8v/checkout/cs_dd293263-537c-dd22-78ab-072adcc1624f | **200** |

**Summary:** 3/3 products confirmed in PRODUCTION, 3/3 fresh real-money checkout links minted, 3/3 links return HTTP 200. No failures.

---

## Registry changes

File: `product-factory/waffo_product_registry.json`

- Added to each of the 3 R11 winner entries (keyed by `PROD_` id) under `published`:
  - `"prodPublished": true`
  - `"prodCheckoutUrl": "<url above>"`
  - `"prodLaunchedAt": "2026-07-13T16:19:32.335Z"`
- Added top-level key `r11WinnersProduction` (launchedAt, status, note, products[]).
- Appended a production note to the `notes` array.
- **Untouched:** the other 73 published entries and the 3 `failed` entries (verified: `failed` count = 3, `published` count = 76, unchanged).

---

## ⚠️ Required follow-up — Umami revenue tracking NOT yet live

Purchase events will **not** appear in analytics/revenue dashboards until these three environment
variables are set in the deployed app and it is **redeployed**:

- `NEXT_PUBLIC_UMAMI_URL`
- `NEXT_PUBLIC_UMAMI_ID`
- `WAFFO_WEBHOOK_SECRET`  (webhook env: `WAFFO_WEBHOOK_PROD_PUBLIC_KEY`)

Event names `order.completed` / `subscription.activated` / `subscription.payment_succeeded` are
shared between test and prod, so once the secret is wired and the app redeployed, prod purchases
will start flowing into Umami automatically. **Until then, real purchases occur but are untracked.**

---

## Safety attestation

- ✅ Canary-first: `privacyscan` verified (HTTP 200) before `aiactradar` / `agentredteam`.
- ✅ No purchase / real payment was ever executed. Verification = checkout page loads HTTP 200 only.
- ✅ Only the 3 R11 winner products were modified.
- ✅ Keys read from `E:/AgentCPM/_keys.env`; no secrets written to disk or logs beyond the public checkout URLs.

## Artifacts

- Launch script: `product-factory/r11_prod_launch.mjs`
- Registry updater: `product-factory/_update_registry.cjs`
- Raw result (last run, 2 products): `product-factory/_r11_prod_result.json`
- Updated registry: `product-factory/waffo_product_registry.json`
- This report: `opc-doc/outputs/08-asset/r11-prod-launch-report.md`
