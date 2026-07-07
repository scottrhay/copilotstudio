# Lab 3 — Draft a clean ticket, and act on its own

**Course:** PL-7008 — Create agents in Microsoft Copilot Studio
**Session:** 2
**Time:** ~30 minutes (two parts)

> **Lab Scenario.** Topics made the agent *predictable*; now you'll make it *capable* and *proactive*. First you'll teach it to turn a messy IT complaint into a **clean, structured ticket** — using nothing but its **instructions**, so it works in any tenant. Then you'll wire an **event trigger** so it reacts to an incoming email **without being asked** — the autonomous behavior **Agent Builder can't do.**

> **Note on the AI Prompt tool.** Copilot Studio can also package drafting logic as a reusable **Prompt tool**. That feature depends on the **AI Prompts** capability being enabled in your environment (admin-controlled) — in many tenants it's turned off, and the Prompt option shows as **disabled** when you try to use it. This lab uses the **instruction-based** path instead: it needs no extra features, works in any tenant, and is more deterministic. If your tenant *does* have AI Prompts enabled, Stretch A shows the Prompt-tool version.

## Quick start
- Open your **HP Workplace Assistant** (from Labs 1–2). Any agent with **generative orchestration** on works — triggers require it, and it's the default.
- No files to download. Two parts: **A — teach it to draft**, **B — add a trigger.**

---

## Part A — Teach the agent to draft a clean ticket

You don't need a special tool to make the agent produce structured output — its **instructions** do it. You'll add a rule that turns any freeform IT problem into a Title / Category / Summary.

### Step 1 — Add the drafting instruction
1. Open your agent → **Overview → Instructions → Edit**.
2. In your **Step-by-Step Instructions** section (under how you handle an IT request), add this, then **Save**:
   ```
   When a user describes an IT problem in their own words, turn it into a clean support ticket and show it to them. Output exactly:
   - Title: a one-line summary
   - Category: one of Access, Hardware, Software, Network, Other
   - Summary: two sentences a technician can act on
   Ignore requests that aren't IT-related.
   ```
3. **Publish** the agent.

### Step 2 — Test the drafting
1. Open the **Test** pane → **Show activity map** On → **Start new test session** (**+**).
2. Enter a messy description:
   ```
   outlook keeps asking me to sign in over and over since this morning
   ```
3. Confirm the agent returns a clean **Title / Category / Summary**. That structured draft is exactly what Lab 4's flow will log and route. *(The agent did this from its instructions alone — no AI Builder, no extra tool, works in any tenant.)*

> **Topic vs. instruction — who does what.** The **Log IT Request topic** (Lab 2) is *structured intake* — it walks the user through name, issue, and urgency step by step. This **drafting instruction** turns a *freeform* description into a ticket. Tell the agent when to use each — in the same **Step-by-Step Instructions** section: *"Use the drafting rule when the user simply describes the problem; use the Log IT Request topic when they want to formally log or submit it."*

---

## Part B — Add a trigger: make the agent act on its own

A **topic trigger** waits for a user to type. An **event trigger** fires on something happening in another system — an email arriving, a file created, a schedule — so the agent runs **autonomously.** *(Event triggers need generative orchestration (on by default), run on **your** credentials, and can affect billing — fine for this lab, worth knowing for production.)*

### Step 3 — Add the trigger
1. **Overview** page → **Triggers** section → **Add trigger**.
2. Choose **When a new email arrives (V3)** on the **Office 365 Outlook** connector. Sign in when prompted (this uses your account). *(The "(V3)" is just the connector version — pick that one.)*
   > No inbox to spare? Choose **Recurrence** instead (e.g., every 1 hour) — the rest of the lab is the same, minus the email content.
3. **Scope it so it doesn't fire on every email** — each match is billable. Set **Folder** = **Inbox** and **Subject Filter** = `IT:`; leave the rest blank. Now the agent only wakes for mail whose subject contains `IT:`.
4. **Next** → keep the default payload (it hands the agent the email's subject and body) → **Finish**.

### Step 4 — Tell the agent what to do when it fires
1. Your drafting rule already knows how to build a ticket; add a trigger-specific line — **Overview → Instructions → Edit**, add and **Save**:
   ```
   When an email arrives about an IT problem, draft a clean ticket from the email body and show the resulting ticket. Ignore emails that aren't IT-related.
   ```
   *(You'll add a Teams post or a flow in Lab 4 — for now the agent just drafts the ticket.)*
2. **Publish** the agent — a trigger only runs after publishing.

### Step 5 — Fire it and watch
1. Send yourself an email — subject `IT: VPN down` (it must contain your `IT:` filter token), body `I can't connect to the VPN from home and need it for a 2pm meeting.`
2. Open the **Activity** page (left nav). Within a minute or two you'll see the trigger fire and the agent's reaction — the ticket it drafted — recorded step by step.
   > Triggers can take a moment. If nothing shows during class, open **Activity** and talk through it: the agent ran **with no one chatting to it.** That's an autonomous agent — impossible in Agent Builder.

---

## ⭐ Finished early? Optional stretch goals

### Stretch A — Package the drafting as a reusable Prompt tool *(only if your tenant has AI Prompts enabled)*
Instructions live on one agent; a **Prompt tool** is a reusable, shareable drafting action you can drop into any agent. **This needs the AI Prompts feature turned on in your environment — if the Prompt option is missing or shows "disabled," skip this stretch; the instruction path you built already does the job.**
1. **Tools** tab → **+ Add a tool** → **+ New tool** → **Prompt**.
2. Name it `Draft IT Ticket`. Paste the same drafting logic (Title / Category / Summary). Add a **Text** input named `IssueText` — put your cursor where the request goes, type **`/`** → **Input → Text**, rename the pill to `IssueText`, and give it sample data.
3. **Test** → **Save** → **Add and configure**. Add an instruction telling the agent to use `/Draft IT Ticket`, then test. Same result — now reusable across agents.

### Stretch B — A connector tool
Tools also come from connectors — no AI Prompts needed.
1. **Tools** → **+ Add a tool** → **Connector** → search a no-auth service (e.g., **MSN Weather → Get current weather**) → add it, name it clearly, **Add to agent**.
2. Instruction: `When asked about the weather, use the weather tool.` Test `what's the weather in Boise?` — a real tool the agent picks by description, fills the input, and acts.

### Stretch C — Make the trigger precise
1. Edit the trigger's instructions so it only acts on **urgent** emails (e.g., subject contains `urgent`) and does nothing otherwise.
2. Send one urgent and one normal email; confirm in **Activity** that only the urgent one triggered a reaction. Tight conditions are how autonomous agents stay useful instead of noisy.

---

## ✅ Done — and how to reuse it
Your agent now **drafts structured tickets from plain language** and **acts on its own** when email arrives — the autonomy that separates Copilot Studio from Agent Builder. **Reuse idea:** a drafting instruction is the fastest way to add a repeatable format (summaries, classifications, drafts); an event trigger turns any agent into a background worker that watches a mailbox, a list, or a schedule.

| Check | |
|---|---|
| Agent **drafts a clean Title / Category / Summary** from a freeform problem | ☐ |
| An **event trigger** added (email or recurrence) and the agent **published** | ☐ |
| Trigger fired and the drafted ticket appears on the **Activity** page | ☐ |

*Next lab: build a **flow** — the richest tool type — and have the agent log and route the ticket it drafts.*
