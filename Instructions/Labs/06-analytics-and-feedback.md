# Lab 6 (optional) — Measure it: analytics & feedback

**Course:** PL-7008 — Create agents in Microsoft Copilot Studio
**Session:** 2 (Operate)
**Time:** ~15 minutes — **optional**

> **Lab Scenario.** A published agent quietly collects data on how it's doing — usage, answer quality, which knowledge sources it leans on, and thumbs-up/down from real users. This short activity shows you where to look, how to leave feedback, and — importantly — **why the Analytics tab looks empty right after you've been testing.**

> **Read this first — why your Analytics is empty.** The **Analytics** page **ignores everything you do in the Test pane.** It only counts real conversations on a **published channel** — like the **Demo website** you published in Lab 1. And even those take **up to about a day** to aggregate (the data updates daily, in UTC). So this is a *seed-it-now, read-it-tomorrow* activity. Some tiles also need volume: answer-**quality** scoring needs at least **10 answers a day**, and the **AI Summary** and **Sentiment** tiles need enough traffic before they show anything.

## Quick start
- Use your **HP Workplace Assistant** from Lab 1 — it must be **published** with a **Demo website** channel.

---

## Your turn

### Step 1 — Generate real usage (on a published channel, not the Test pane)
1. **Channels** tab → **Demo website** → **Open demo website** (or use the published link).
2. Have **3–4 real conversations** in that window — ask a few IT questions the FAQ can answer (`How do I connect to the VPN?`, `How do I reset my password?`) and one it can't (`How much vacation do I get?`).

### Step 2 — Leave feedback
1. Under a couple of the agent's answers, click **thumbs up** or **thumbs down**. That's the **satisfaction** signal analytics reports, and thumbs-down sessions are the ones you'll want to review later.

### Step 3 — See it immediately in Activity
1. Back in Copilot Studio, open the **Activity** tab *(called **Monitor** in some tenants)*.
2. Your demo-website sessions show up here **quickly** — select one to see the full transcript, which knowledge sources it used, and any tools it called. This is the real-time view; **Analytics** is the aggregated, next-day view.

### Step 4 — Tour the Analytics tab
1. Open the **Analytics** tab and walk the sections:
   - **Overview** — conversation sessions and engagement.
   - **Effectiveness → Conversation outcomes** — Resolved / Escalated / Abandoned / Unengaged.
   - **Use → Generated answer rate and quality** — how many questions got answered, and how good the answers were (needs ≥10 answers/day to score).
   - **Use → Knowledge source use** — which documents actually get used (your IT Support FAQ shows here).
   - **Sentiment**, **Billing** (Copilot credits), and **Savings**.
2. **Expect zeros today** — that's normal. The demo-website sessions and thumbs feedback from Steps 1–2 will appear **tomorrow**. Note where you'd look for each once data lands.

---

## ⭐ Finished early? Optional stretch goals

### Stretch A — Start a savings (ROI) tile
On the **Analytics** page, find **Savings → Calculate savings** and enter an estimated time saved per resolved question (say, 5 minutes). Copilot Studio computes a running total against successful sessions — a defensible ROI number with no spreadsheet.

### Stretch B — Define a custom metric
Under **Custom metrics**, add one in plain language (e.g., "percent of sessions where the user asked about VPN"). Copilot Studio samples sessions and scores it as a labeled donut — your own KPI, no code.

---

## ✅ Done — and how to reuse it
You now know the difference between the **Activity** view (real-time, per-session) and the **Analytics** view (aggregated, next-day), and how thumbs-up/down feeds satisfaction. **Reuse idea:** once your agent is live, check **answer quality** and **knowledge source use** weekly, and open every **thumbs-down** session — those are your fastest path to a better agent.

| Check | |
|---|---|
| Had real conversations on the **Demo website** (not the Test pane) | ☐ |
| Left at least one **thumbs up / thumbs down** | ☐ |
| Found the session in the **Activity** tab with its transcript | ☐ |
| Toured the **Analytics** tabs and know the data lands ~next day | ☐ |

*You've built an agent end to end, and now you know how to measure it. That's the full loop: create → ground → structure → act → compose → operate.*
