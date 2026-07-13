# R15 Screening & Selection — Real Estate Tech (PropTech) Cluster

**Round:** 15 · **Worker:** r15-dev-round · **Date:** 2026-07-13
**Chosen niche:** AI-assisted copy & document tools for **real estate agents, landlords, and property managers** (PropTech / Real Estate Tech).

---

## 1. Market signal table (WebSearch sources)

`last30days` skill is interactive (needs AskUserQuestion + external API) and cannot run
in this unattended sandbox, so per prior authorization I fell back to **WebSearch** to
evaluate trend / growth / competition signals for candidate niches.

| Signal / metric | Source | Finding for Real Estate Tech |
|---|---|---|
| Market size & growth | [superframeworks.com — Profitable Micro SaaS Niches 2026](https://superframeworks.com/articles/profitable-micro-saas-niches) | Real Estate Tech listed as a top vertical: **$3.2B by 2030, 17% CAGR**, "Medium" competition, entry barrier **Low**, avg pricing **$39–$299/mo**. Example: Follow Up Boss, kvCORE, Dotloop. |
| Demand / willingness to pay | same | "Real estate agents earn commission-based income and readily invest in tools that generate leads and close deals faster. 1.5M active US agents. Highly referral-driven → fast word-of-mouth." |
| VC funding concentration | [deepresearch.ninja — Niche SaaS Opportunity Map 2026](https://deepresearch.ninja/2026/05/The-Niche-SaaS-Opportunity-Map-2026) | Real Estate shows **51% early-stage concentration** — greenfield, not yet consolidated. |
| Micro-SaaS fit | [nxcode.io — 50 Micro SaaS Ideas 2026](https://www.nxcode.io/resources/news/micro-saas-ideas-2026) | "AI Email Writer for Real Estate Agents" explicitly called out as a proven $19–$39/mo idea (buyer follow-ups, listing descriptions, negotiation emails). Vertical CRMs for real estate agents outperform generic tools. |
| Category saturation check | [bigideasdb.com — SaaS Market Trends 2026](https://bigideasdb.com/saas-market-trends-2026) | Pure "AI tools" are oversaturated (1,213 startups, median MRR $7). **Vertical, text-native tools in less-crowded categories win.** Real estate copy/docs is vertical, not generic-AI, and text-native. |

**Decision rationale:** Real Estate Tech is (a) **rising** (17% CAGR, 51% early-stage funding),
(b) **low competition / low entry barrier** vs saturated "AI tool" category, (c) **text-native**
(listing copy, lease clauses, bios, follow-ups, CMA narratives, neighborhood blurbs — all LLM
text rendered to a page), and (d) **zero overlap** with existing factory clusters.

---

## 2. Overlap check vs R1–R14 (must be NEW & non-overlapping)

| Existing cluster | Scope | Overlap with R15? |
|---|---|---|
| R11 AI governance/security/privacy (aiactradar, agentredteam, privacyscan, promptshield, litwatch…) | AI risk/compliance | None |
| R12 Developer tools (loginsight, regexforge, jsonschema, diffexplain, apimock, envscan, typegen, cihelper) | Dev/infra | None |
| R13 E-commerce/shop ops (prodcopy, variatcopy, reviewminer, sizeguide, returncopy, upsellideas, storeseo, bundler) | Online sellers | None (prodcopy is shop product copy, not real estate) |
| R14 HR-tech/recruiting (jobdesc-ai, interviewq, candidatescore, offerletter, rejectmail, onboardplan, compreason, recruiterly) | Hiring | None |
| Other (resumeforge, coldmail/email tools, briefkit, fitness/health, legal policyforge/clauseguard) | jobseeker / cold email / SEO / fitness / legal | None |

`agentledger` and `agentredteam` exist in the dir but are AI-agent / security products, not real estate.
**Result: no overlap.** All 8 R15 slugs verified free in both `12_Micro_SaaS出海/` and `waffo_product_registry.json`.

---

## 3. Selected 8-product cluster (shared buyer: real estate agents / landlords / property managers)

| # | Slug | Category | One-line value | Audience | Free tier | Pro tier |
|---|---|---|---|---|---|---|
| 1 | `houselist-ai` | Listing copy | MLS-ready residential listing description with hook + benefits | Listing agents | $0 · 3/mo | $19/mo · unlimited, save history |
| 2 | `leasecraft` | Lease clauses | Plain-English, state-aware lease clauses to drop in | Landlords / PMs | $0 · 5/mo | $15/mo · unlimited, save history |
| 3 | `staydesc` | Rental copy | Airbnb/Vrbo descriptions guests actually book | Short-term hosts | $0 · 4/mo | $19/mo · unlimited, save history |
| 4 | `agentbio-ai` | Agent bio | Confident first-person agent bio for site/Zillow | Agents | $0 · 3/mo | $15/mo · unlimited, save history |
| 5 | `leadfollow` | Follow-up email | Post-showing & nurture emails that stay top of mind | Agents | $0 · 5/mo | $19/mo · unlimited, save history |
| 6 | `cmanarrative` | CMA narrative | Turn comps into a pricing story sellers believe | Listing agents | $0 · 4/mo | $19/mo · unlimited, save history |
| 7 | `neighborhoodguide` | Neighborhood copy | Lifestyle blurbs that make location the hero | Agents / relocation | $0 · 4/mo | $15/mo · unlimited, save history |
| 8 | `homeprep-ai` | Staging/prep plan | Prioritized home-prep & staging checklist before photos | Sellers / agents | $0 · 4/mo | $15/mo · unlimited, save history |

All 8 share one buyer and cross-promote (agent uses houselist → cmanarrative → leadfollow →
agentbio → neighborhoodguide → homeprep; landlord uses leasecraft + staydesc).

---

## 4. Hard-rule compliance confirmation

- **Exactly 2 pricing tiers** (Free $0 + Pro) on every product. No Team/Studio/Enterprise. ✅
- **`features` list only implemented capabilities** (LLM text rendered to page). No export /
  version history / API access / team workspace / priority support / collaboration claimed. ✅
- **Pricing descriptions say "save history", never "export".** ✅
- **Zero CJK** in all generated files (English UI/code only); slugs ASCII. ✅
- **Real `systemPrompt`** on every product so `/api/tool.ts` returns useful output. ✅
- **Globally unique slugs** — verified free in `12_Micro_SaaS出海/` and registry. ✅
- **`--name <slug>`** used in deploy so project name == slug. ✅
- No overlap with R1–R14 clusters. ✅
