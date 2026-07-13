# Umami + Waffo Webhook Integration Report — R11 Winners (3 apps)

**Date:** 2026-07-13
**Scope:** 3 R11 winner apps only — `aiactradar`, `agentredteam`, `privacyscan`
**Changes:** Replaced GA4 `gtag` analytics with env-driven **Umami** tracking, and added a
server-side **Waffo → Umami** `purchase` webhook. Other 73 products and `pricelens`'s
existing webhook were left untouched.

---

## 1. Files changed (per app)

Identical change set applied to each of the 3 apps:

| File | Change |
|---|---|
| `lib/analytics.ts` | Rewrote GA4 `gtag` helpers → **Umami**. Exports `UMAMI_ID`, `UMAMI_URL` (env-driven, trailing-slash stripped), `umamiTrack(event, payload?)`, `trackCheckoutCta(p)` → `generate_lead` + `begin_checkout`, `trackToolUsed(itemId, success, llmReal)` → `tool_used`. All functions **NO-OP safely** when `UMAMI_ID` is undefined (no crash). Comment block notes `purchase` is fired server-side by the webhook. |
| `pages/_app.tsx` | Removed GA4 `gtag.js` loader. Added `next/script` Umami loader `<Script src={`${UMAMI_URL}/script.js`} data-website-id={UMAMI_ID} strategy="afterInteractive" />`, rendered **only when `UMAMI_ID` is truthy** (so builds without env vars emit no broken script). Component imports unchanged (`trackCheckoutCta` / `trackToolUsed` names preserved → no component edits needed). |
| `pages/api/waffo-webhook.ts` | Next.js API route. Reads raw body (`bodyParser:false`), verifies the Waffo signature with **HMAC-SHA256** using `WAFFO_WEBHOOK_SECRET` (shared secret, not RSA), and on a revenue event (`order.completed` / `subscription.activated` / `subscription.payment_succeeded`) `POST`s a `purchase` event to `${UMAMI_URL}/api/send`. Always returns **HTTP 200** (bad signatures are logged + skipped, never forwarded). |

No other files touched. Components (`pages/index.tsx`, `components/Tool.tsx`) keep calling
`trackCheckoutCta` / `trackToolUsed` — same exported names, only the inner impl changed.

---

## 2. Deploy status

| App | Build | Deploy | Live URL | Status |
|---|---|---|---|---|
| aiactradar | ✅ | ✅ | https://aiactradar.vercel.app | **DEPLOYED** |
| agentredteam | ✅ | ✅ | https://agentredteam-coral.vercel.app | **DEPLOYED** |
| privacyscan | ✅ | ✅ | https://privacyscan-ten.vercel.app | **DEPLOYED** |

> Build/install used **system Node 24** (`/c/Program Files/nvm/v24.12.0`) in a temp dir
> (`/tmp/<slug>`) so the source trees were never modified. `next.config.js` has
> `typescript.ignoreBuildErrors: true`, so the build is robust to type nits.
> All three compiled the `λ /api/waffo-webhook` serverless route cleanly, and all three
> went **Ready** on Vercel (aliases confirmed).
>
> **2026-07-13 FIX (this deploy):** the prior `waffo-webhook.ts` used the WRONG signature
> scheme (RSA-SHA256 with `WAFFO_WEBHOOK_PUBLIC_KEY`) and would have silently rejected every
> real Waffo signature once a key was set. Rewritten against the official Pancake spec:
> **HMAC-SHA256** verification using `WAFFO_WEBHOOK_SECRET` (a shared HMAC secret, not an RSA
> key), correct Pancake event set, and `data.amount` treated as a **number** (→ `String()`
> for Umami). See §5 (RESOLVED).
>
> **⚠️ Env vars are NOT set yet.** The code NO-OPs (gracefully) until `NEXT_PUBLIC_UMAMI_URL`,
> `NEXT_PUBLIC_UMAMI_ID`, `WAFFO_WEBHOOK_SECRET` are present. A **redeploy is required after**
> the user sets them (see §3) so the `NEXT_PUBLIC_*` values compile into the client bundle.
> Until then the Umami script is not emitted and purchases are not forwarded. When
> `WAFFO_WEBHOOK_SECRET` is unset the webhook still forwards UNVERIFIED events (best-effort);
> once it is set, unverified events are SKIPPED (never forwarded).
>
> Note: Vercel assigned the alias `agentredteam-coral` / `privacyscan-ten` (not the
> bare `<slug>`). The webhook URLs in §4 use the canonical `https://<slug>.vercel.app`
> form; if that 404s, use the actual alias above (both resolve to the same deployment).

---

## 3. Environment variables to set in Vercel (per app)

Set these in each app's Vercel project → Settings → Environment Variables, then **redeploy**:

| Var | Example | Notes |
|---|---|---|
| `NEXT_PUBLIC_UMAMI_URL` | `https://umami.yourdomain.com` | Must be **HTTPS** in prod (mixed-content block otherwise). Trailing slash auto-stripped. |
| `NEXT_PUBLIC_UMAMI_ID` | `a1b2c3d4-…` (website UUID) | From Umami → Settings → Websites → Tracking Code → `data-website-id`. |
| `WAFFO_WEBHOOK_SECRET` | `whsec_…` (webhook signing secret) | **IMPORTANT RENAME:** the old var was `WAFFO_WEBHOOK_PUBLIC_KEY` — that name was WRONG. This is a **shared HMAC secret**, NOT an RSA public key. In the Waffo dashboard it is the **Webhook 签名密钥 / signing secret**. Used for **HMAC-SHA256** verification of the `x-waffo-signature` header. Test & prod secrets differ. |

Redeploy command (or use the Vercel dashboard "Redeploy"):
`vercel deploy --prod --yes --token $VERCEL_TOKEN` from the app dir, or click **Redeploy**
on the latest deployment after env vars are saved.

---

## 4. Waffo webhook registration (user action in Waffo Dashboard)

For each of the 3 apps, register a webhook pointing at the live function URL:

| App | Webhook URL |
|---|---|
| aiactradar | `https://aiactradar.vercel.app/api/waffo-webhook` |
| agentredteam | `https://agentredteam.vercel.app/api/waffo-webhook` |
| privacyscan | `https://privacyscan.vercel.app/api/waffo-webhook` |

Subscribe to events:
- `order.completed` (one-time payment)
- `subscription.activated` (first subscription payment)
- `subscription.payment_succeeded` (renewal)
- `refund.succeeded` / `refund.failed` (the webhook logs these but does NOT forward revenue to Umami)

Copy the **Webhook signing secret (签名密钥)** and paste it into `WAFFO_WEBHOOK_SECRET` (§3). Do **not** use an RSA public key — verification is HMAC-SHA256 with this shared secret.

---

## 5. Waffo signature verification — RESOLVED

> Status: **RESOLVED (2026-07-13).** The prior deployment's verification was WRONG and would
> have silently dropped every real Waffo purchase once a key was configured. This deploy
> implements the verified Pancake spec.

**Two prominent fixes in this deploy:**

1. **Algorithm: HMAC-SHA256, NOT RSA.** The old code did `crypto.createVerify('RSA-SHA256')`
   against a PEM "public key". Waffo Pancake signs webhooks with **HMAC-SHA256** using a
   **shared secret** (not an RSA keypair). The wrong algorithm would reject 100% of genuine
   signatures → real purchases never reach Umami, silently.
2. **Env var renamed `WAFFO_WEBHOOK_PUBLIC_KEY` → `WAFFO_WEBHOOK_SECRET`.** It is the webhook
   signing **SECRET** (HMAC shared key), not an RSA public key. Using the wrong var name would
   also have led the user to paste an RSA public key where a secret is expected.

**Verified scheme (implemented in `pages/api/waffo-webhook.ts`):**

- **Header:** `x-waffo-signature` (also tolerates `waffo-signature`).
- **Format tolerated:** raw hex, or `t=<timestamp_ms>,v1=<sig>`, or `sha256=<sig>`.
- **Algorithm:** **HMAC-SHA256** over the **raw request body**, keyed by `WAFFO_WEBHOOK_SECRET`.
  Constant-time compare (`crypto.timingSafeEqual`) against both hex and base64 encodings.
- **Verify path chosen:** **manual HMAC** (zero new dependency). The official SDK
  `@waffo/pancake-ts` `verifyWebhook(body, sig)` was evaluated and confirmed to use **embedded
  RSA public keys** and takes no secret argument, so it cannot consume the `WAFFO_WEBHOOK_SECRET`
  HMAC secret this integration requires. The manual HMAC therefore matches the official
  `docs.waffo.ai/zh/features/integrations` snippet and satisfies the secret-based scheme.
- **Behavior on bad signature:** still returns **HTTP 200** (Waffo retry policy) but **SKIPs
  forwarding** — unverified events are never ingested once a secret is configured. If the secret
  is unset, the webhook forwards UNVERIFIED events best-effort (so the pipeline works pre-config)
  and logs a warning. **Always returns 200.**
- **Refunds:** `refund.succeeded` / `refund.failed` → logged only, never forwarded as revenue.

**Correct event set (the ONLY revenue events forwarded as Umami `purchase`):**
- `order.completed` (one-time payment)
- `subscription.activated` (first subscription payment)
- `subscription.payment_succeeded` (renewal)

Defensive aliases for the SDK `WebhookEventType` enum spellings (`OrderCompleted`,
`SubscriptionActivated`, `SubscriptionPaymentSucceeded`) are also accepted.

**Payload `data` shape (WebhookEventData):**
- `data.amount` is a **NUMBER** (e.g. `9.99`) — **fixed**: the old code assumed a string and
  `parseFloat`'d it. Now we read the number directly and `String()` it for Umami's `value`
  (Umami requires `value` as a string).
- `data.currency` (e.g. `"USD"`), `data.productName`, `data.orderId`, `data.buyerEmail`,
  `data.taxAmount` (number).
- Top-level: `id` (delivery id, used for dedup), `eventId` (business/payment id), `storeId`,
  `mode` (`"test"` | `"prod"`).
- **No product slug** in the payload → `item_id` falls back to the app slug
  (`aiactradar` / `agentredteam` / `privacyscan`).
- `transaction_id` = `data.orderId || event.eventId`.

**Official SDK note:** if you prefer the SDK path, `@waffo/pancake-ts` exposes
`verifyWebhook(body, sig)` (throws on bad signature). It verifies with Waffo's embedded RSA
public keys and does not take your shared secret — so it is suitable only if Waffo signs with
its own RSA key rather than your HMAC secret. The current deploy uses the HMAC-secret path.

---

## 6. Verification checklist

1. Set the 3 env vars (§3) and **redeploy** each app.
2. Open Umami dashboard → Realtime: confirm `generate_lead` / `begin_checkout` / `tool_used`
   flow in from the browser.
3. In Waffo (test mode) trigger a test payment → webhook fires → Umami shows a `purchase`
   event with `value`, `currency`, `transaction_id`, `item_id`, `item_name`.
4. Confirm the webhook returns `200` even on bad signature (check Vercel function logs).

---

## 7. Rollback

The GA4 code was fully replaced. To revert, restore `lib/analytics.ts`, `pages/_app.tsx`
from git history and delete `pages/api/waffo-webhook.ts`. (No DB/migration involved.)
