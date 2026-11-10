# Prototype Notes — Ruta Express Claims Console (throwaway)

**Author:** Cristhian Ruiz, AI PdM · **Date:** 2026-11-05 · **Tool:** Claude artifact (single HTML page, no backend) · **Time spent:** ~50 min inside the 60-min box (build 35, screenshots and functional check 10, this note 5)
**Live:** [`index.html`](../index.html) (served by GitHub Pages when enabled) · **Screenshots:** [`prototype/screenshots/`](../prototype/screenshots/) · **Source:** [`prototype/claims_agent_console.html`](../prototype/claims_agent_console.html)

## What it shows

Two claim types (PdM decision 5A) in the two autonomy modes the Opportunity Brief specifies:

- **Late delivery — resolved by the agent alone** (I-2): three synthetic claims; the agent reads the promised window and delivery time, applies the 24 h / 10%-of-shipping-fee rule (T L08), issues the credit or writes the "no credit" reply (EC1, EC2), and closes. A person can reopen.
- **Damaged — a person decides** (I-3, I-4): three synthetic claims; the agent checks the photo window (48 h policy / 72 h practice, EC3, EC4), proposes an amount up to the declared value (T L10), routes above-cap and above-threshold cases to an agent or a supervisor (EC8, EC9), drafts the reply, and waits. Approve / reject buttons record the decision and the simulated PayCo and Helpdesk calls in a log.
- **Policy settings** (cap per currency, supervisor threshold, photo window, late threshold, credit %) that the agent reads and never changes; changing one re-evaluates every claim live (AC6).
- **KPIs** on the queue: resolved with no human action, money paid by the agent alone, automatic payments above cap (always 0).
- **Customer reply** in formal "usted" (T L24, AC4), labelled "draft, sent only after approval" when a person is in the loop.

## What it taught me about the WHAT

1. **The agent's output is a case file, not a verdict.** Showing the reasoning steps with their rule citations next to the proposed decision is what makes a 20-minute damaged claim a 2-minute approval. The minutes move from *investigating* to *checking*; that is the mechanism behind KR1, and it works even when the person stays in the loop. The brief's "prepare every other claim" clause (I-3) is where most of the value sits, not in the agent-alone share.
2. **Settings are the product.** Three of the four open conflicts (photo window, supervisor threshold, GPS buffer) disappear from the build once they are settings with an owner. In the prototype, moving the cap from 100,000 to 800 COP moves two late claims from "resolved alone" to "needs approval" instantly, with no code. The CFO's cap and the VP's autonomy wish are the same dial.
3. **The 24 h rule produces a second kind of "resolved alone": the no-credit reply.** 2 of 7 late claims in the export (E: CLM-2005, 2015) are ≤ 24 h. The agent closes them with an explanation; that is still a human minute saved and still zero money. Day-one autonomy counts both.
4. **The 49–72 h photo case needs a name on screen.** The flag "outside policy, inside practice" is the only thing stopping an agent-user from rejecting a claim their team would have accepted yesterday. Change management (Mariana, Q1) is a UI label before it is a policy memo.
5. **Money and minutes are different problems.** The agent alone paid 2,100 COP across six synthetic claims; the damaged queue holds 1.59 M COP waiting for people. The agent-alone scope is nearly free of financial risk; the human-in-the-loop scope is where fraud and error controls matter (T L16, L35).

## What it does not prove

- **No handle-time measurement.** The prototype has no timer and no users; AC1 is measured in S3 with agents on a synthetic set, not here.
- **No triage.** Claim type is given; EC10 (missing or inconsistent type) is not exercised. Triage from free text is the first S3 unit of work.
- **No lost or wrong-address flows** (EC5, EC6, EC7); no Mexico queue (EC13); no business-account priority ordering (EC12) beyond a label.
- **No photo analysis.** "Photo shows damage" is a synthetic boolean; in S3 it is a model call with its own error rate and cost telemetry (AC9).
- **No throughput.** Six claims, one browser; nothing about 4,800 a month or same-day answers at volume.
- **Nothing about the customer channel.** The reply is a draft in a panel; ReclamaYa, email and WhatsApp (out of scope) are not represented.

## What in it is simulated

| Element | Real in production | In the prototype |
|---|---|---|
| Claims | ReclamaYa → Helpdesk ticket (I:9) | 6 invented records `SYN-3001…3006`; no name, phone, email or address field exists; amounts and timings mirror patterns in the export without copying any row |
| Order data | Atlas REST API (I:11) | Hard-coded fields on each claim |
| Tracking / GPS | Rastreo feed (I:12) | "hours after window" as a number |
| Photo | ReclamaYa attachment | A one-line note and a boolean |
| Payment | PayCo API (I:13) | A log line "PayCo (simulated) refund …" |
| Reply delivery | Helpdesk (I:10) | A log line; nothing is sent |
| Agent reasoning | LLM calls with telemetry | Deterministic JavaScript rules; same decisions, no tokens |
| Settings | Client-owned configuration | Form inputs in the page |
| Users / roles | Agent vs supervisor logins | One view; "supervisor" is a label on the decision |
| Timestamps | System clock in CO/MX | Browser UTC time |
