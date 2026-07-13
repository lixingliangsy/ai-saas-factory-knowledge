# R11 Winners — Value Propositions & Landing Copy
**Products:** AIActRadar · PrivScan · AgentRedTeam  **Owner:** 言之 (opc-value-designer)

> **Framing rules applied.** Every claim is grounded in what the live LLM actually outputs — a real-time, AI-generated report, not templates or static samples (the `/api/tool.ts` default routes to NVIDIA NIM; 3 env vars make it production-real). Buyer = EU/US SMB, indie developer, or compliance lead (B2B, never consumer). Tone = professional, trustworthy, lightweight; the three share one AI-governance cluster and can cross-promote. Regulatory maximums and the IBM breach figure are cited as statutory/public reference points, not promises of outcome. None of the copy replaces a lawyer, guarantees non-breach, or substitutes for a formal audit.

---

## 1. AIActRadar — $59/mo
**Tagline:** Map your AI to its EU AI Act obligations in minutes.

**Target customer:** EU-facing startups and SMBs shipping AI features who must know their AI Act exposure without hiring a compliance consultant.

**Core selling points**
- **Risk tier in plain language** — The AI classifies your system as unacceptable / high / limited / minimal risk with a one-line rationale, so you immediately know how seriously to treat compliance. *Basis: risk-tier classification step in the live prompt.*
- **Obligations + deadlines mapped to you** — You get the specific articles (Art. 9 risk management, Art. 11 technical docs, Art. 14 human oversight) and the 2 Aug 2026 high-risk deadline that apply to your deployment — not a generic checklist. *Basis: obligations + key-deadlines steps.*
- **Hand-off-ready action checklist** — Output is a prioritized required-actions list your engineering or legal team can execute this week. *Basis: required-actions checklist step.*

**ROI hook:** EU AI Act fines reach €35M or 7% of global turnover; at $59/mo AIActRadar costs less than one hour of counsel to stay ahead of every deadline.

**Landing hero (EN)**
- Headline: *Know your EU AI Act obligations before the deadline does.*
- Subheadline: *Describe your AI system and get a real-time risk classification, the exact articles you must meet, and a prioritized compliance checklist — no legal retainer required.*

---

## 2. PrivScan — $29/mo
**Tagline:** Catch GDPR gaps on your site before regulators do.

**Target customer:** SMBs and indie developers with EU/UK users who need a fast, affordable privacy health-check without a DPO or outside audit.

**Core selling points**
- **Readiness score in one scan** — The AI returns a 0–100 privacy score mapped to GDPR articles (Art. 6 lawful basis, Art. 7 consent, Art. 13/14 transparency) so you see where you stand at a glance. *Basis: privacy-readiness score step.*
- **Issues tied to specific articles** — Every gap is cited to the regulation (e.g. granular consent under Art. 7), so fixes map to legal requirements, not guesswork. *Basis: article-mapped issues step.*
- **Remediation checklist your dev team can ship** — Concrete required fixes (granular opt-in, lawful-basis statements, retention notices) ready to hand to engineering. *Basis: required-fixes + remediation steps.*

**ROI hook:** GDPR penalties reach €20M or 4% of annual revenue; PrivScan's $29/mo finds the consent and disclosure gaps that trigger them — long before a complaint lands.

**Landing hero (EN)**
- Headline: *Find your GDPR gaps before a regulator or a user does.*
- Subheadline: *Enter your site URL and data practices to get a real-time privacy-readiness score, the exact articles you're missing, and a remediation checklist your team can act on today.*

---

## 3. AgentRedTeam — $79/mo
**Tagline:** Break your AI agents before attackers do.

**Target customer:** Builders of autonomous AI agents and copilots (startups, dev teams, AI-forward SMBs) who ship agents with real tool/email/API access and can't afford a silent breach.

**Core selling points**
- **See your exploitability score** — The AI rates how exploitable your agent is (0–100) from its capabilities and threat focus, so you know your real exposure at a glance. *Basis: exploitability-score step.*
- **Attack vectors with real scenarios** — Top 3 plausible attacks (prompt injection via tool output, unconfirmed external sends, PII exfiltration), each with a short example, so engineering sees exactly how a breach could happen. *Basis: top-3 attack-vectors step.*
- **Prioritized hardening checklist** — A ranked fix list (human approval for outbound actions, sanitize tool outputs, PII filters) you can implement this sprint. *Basis: hardening-checklist step.*

**ROI hook:** The average data breach costs $4.9M (IBM 2024); AgentRedTeam's $79/mo surfaces prompt-injection and data-exfiltration risks in your agents before they turn into one.

**Landing hero (EN)**
- Headline: *Find the holes in your AI agent before someone else does.*
- Subheadline: *Describe your agent and its capabilities to get a real-time exploitability score, the top attack vectors with real scenarios, and a prioritized hardening checklist — no security team required.*

---

## Brand & cross-promo note
All three speak in the same voice: specific, calm, evidence-linked (cite the article / score / vector). Each hero subheadline ends by removing a blocker — "no legal retainer / no DPO / no security team" — reinforcing the lightweight positioning. They sit in one AI-governance cluster, so checkout and result pages can cross-link (e.g. AgentRedTeam → AIActRadar for the AI Act angle; PrivScan → AgentRedTeam for agent data-handling). Accuracy guardrails kept throughout: AIActRadar *generates a compliance map/report* (does not replace a lawyer); AgentRedTeam *runs an automated vulnerability scan/report* (does not guarantee the agent cannot be hacked); PrivScan *scans for privacy gaps* (does not issue certifications).
