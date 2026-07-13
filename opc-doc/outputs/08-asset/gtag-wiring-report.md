# GA4 gtag Wiring — Vercel Deploy Report

Generated: 2026-07-13
Scope: 3 R11 winner apps (aiactradar, agentredteam, privacyscan) under `12_Micro_SaaS出海/`
Goal: Ensure the GA4 gtag source (already wired into SOURCE) is LIVE on Vercel.

---

## Source wiring (verified before deploy)
Each app already contained, in SOURCE:
- `lib/analytics.ts` — `GA4_MEASUREMENT_ID = 'G-REPLACE_ME'` placeholder + `gaTrack` / `trackCheckoutCta` / `trackToolUsed` helpers.
- `pages/_app.tsx` — loads `gtag.js` + pushes `dataLayer` events (`generate_lead`, `begin_checkout` on the checkout CTA; `tool_used` after `/api/tool` success; `purchase` is a commented webhook placeholder).

No source files were modified. The `GA4_MEASUREMENT_ID` placeholder remains `'G-REPLACE_ME'` in all three (see note below).

---

## Deploy method
- Toolchain: system Node 24 (`/c/Program Files/nvm/v24.12.0`), `npm.cmd` (managed Node 22 npm is broken).
- Auth: `VERCEL_TOKEN` sourced from `E:/AgentCPM/_keys.env` (account `lixingliangs-projects`).
- For each app: copied SOURCE (excluding `node_modules`/`.next`, keeping `.vercel` for project link) into a clean temp build dir → `npm.cmd install` → `npm.cmd run build` (Next 14) → `vercel deploy --prod --yes --name <slug> --token $VERCEL_TOKEN`.
- Vercel performed its own cloud build from the uploaded source; all three compiled successfully and were aliased.
- Liveness confirmed via `vercel inspect` → status `● Ready` for every deployment (direct `curl` to `*.vercel.app` is blocked by this environment's outbound egress, but the Vercel API confirms the deployments are live and serving).

---

## Per-app results

### 1. aiactradar
- Source wired: YES (`lib/analytics.ts` + `pages/_app.tsx`)
- Deploy status: **DEPLOYED**
- Live deploy URL (alias): https://aiactradar.vercel.app
- Production URL: https://aiactradar-i6yptc21w-lixingliangs-projects.vercel.app
- Inspect: https://vercel.com/lixingliangs-projects/aiactradar/tn7nvj9QjdVxxRTvdG5nTjWopTex
- GA4 ID placeholder: still `G-REPLACE_ME` (awaiting real G-XXXXXXX from user)

### 2. agentredteam
- Source wired: YES (`lib/analytics.ts` + `pages/_app.tsx`)
- Deploy status: **DEPLOYED**
- Live deploy URL (alias): https://agentredteam-coral.vercel.app
- Production URL: https://agentredteam-dcovz17fz-lixingliangs-projects.vercel.app
- Inspect: https://vercel.com/lixingliangs-projects/agentredteam/Ek74UqGqibzyK3roGDJM52bjdsry
- GA4 ID placeholder: still `G-REPLACE_ME` (awaiting real G-XXXXXXX from user)

### 3. privacyscan
- Source wired: YES (`lib/analytics.ts` + `pages/_app.tsx`)
- Deploy status: **DEPLOYED**
- Live deploy URL (alias): https://privacyscan-ten.vercel.app
- Production URL: https://privacyscan-d0ik0s1ju-lixingliangs-projects.vercel.app
- Inspect: https://vercel.com/lixingliangs-projects/privacyscan/24woxNGvFZ74mX1kvcy4oNjPKBDF
- GA4 ID placeholder: still `G-REPLACE_ME` (awaiting real G-XXXXXXX from user)

---

## Closing note
All three apps are **DEPLOYED and live on Vercel with the GA4 gtag source in production**. Analytics will remain **silent until a real GA4 Measurement ID is supplied** — the `G-REPLACE_ME` placeholder must be replaced with a real `G-XXXXXXX` (e.g. via `lib/analytics.ts` or an env var) and redeployed. The `dataLayer`/`event` code (`generate_lead`, `begin_checkout`, `tool_used`, plus the `purchase` webhook placeholder) is correct and ready to fire as soon as a valid ID is in place.

**No other products were touched.** Only `aiactradar`, `agentredteam`, and `privacyscan` were deployed.
