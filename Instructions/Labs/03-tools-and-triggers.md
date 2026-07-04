# Lab 3 — Give the agent tools and a trigger

**Course:** PL-7008 — Create agents in Microsoft Copilot Studio
**Session:** 2
**Time:** ~30 minutes (two parts)

> **Lab Scenario.** Topics made the agent *predictable*; now you'll make it *capable* and *proactive* — two things **Agent Builder can't do at all.** A **tool** lets the agent *do* something: here, an AI **Prompt tool** that turns a messy problem into a clean IT ticket. A **trigger** lets the agent act **without being asked** — you'll wire an event trigger so it reacts on its own. This is the heart of what Copilot Studio adds.

## Quick start
- Open your **HP Workplace Assistant** (from Labs 1–2). Any agent with **generative orchestration** on works — triggers require it, and it's the default.
- No files to download. Two parts: **A — add a tool**, **B — add a trigger.**

---

## Part A — Add a tool: make the agent *do* something (14 min)

A **tool** is how an agent acts. There are several kinds — connectors, workflows, computer use, REST, MCP, and **prompts**. You'll build a **Prompt tool**: pure AI, no external connection or sign-in, so it works in any tenant.

### Step 1 — Create the Prompt tool (7 min)
1. Left navigation → **Tools** → **+ New tool** → **Prompt**.
2. **Name:** `Draft IT Ticket`.
3. In the prompt instructions, paste:
   ```
   You turn a user's rough description of an IT problem into a clean support ticket.
   Output exactly:
   - Title: a one-line summary
   - Category: one of Access, Hardware, Software, Network, Other
   - Summary: two sentences a technician can act on
   ```
4. Add an **input** so the tool can receive the user's text: **+ Add input** → name it `IssueText` (Text). Insert the `IssueText` variable into the prompt where the user's description should go.
5. **Test** the prompt with a sample (`my laptop won't join the wifi and I have a demo in an hour`), confirm it returns Title / Category / Summary, then **Save** (and **Publish** the tool if prompted).

### Step 2 — Attach it to the agent and instruct when to use it (4 min)
1. **Agents** → open **HP Workplace Assistant** → **Tools** tab → **+ Add a tool** → find **Draft IT Ticket** → **Add and configure**.
2. **Inputs → Fill using:** **Dynamically fill with AI** — the agent passes the user's description automatically. **Save**.
3. **Overview → Instructions → Edit**, add and **Save**:
   ```
   When a user describes an IT problem, use the Draft IT Ticket tool to produce a clean ticket, then show it to them.
   ```

### Step 3 — Test the tool, and watch it fire (3 min)
1. **Test** pane → **Show activity map** On → **Start new test session** (**+**).
2. Enter a messy description:
   ```
   outlook keeps asking me to sign in over and over since this morning
   ```
3. The agent calls **Draft IT Ticket** and returns a structured ticket. **In the activity map, watch the tool node fire** — the agent *chose* the tool from its description. That decide-then-do is the tool pattern; Lab 4 builds the richest tool type, a **flow**.

---

## Part B — Add a trigger: make the agent act on its own (14 min)

A **topic trigger** waits for a user to type. An **event trigger** fires on something happening in another system — an email arriving, a file created, a schedule — so the agent runs **autonomously.** *(Event triggers need generative orchestration (on by default), run on **your** credentials, and can affect billing — fine for this lab, worth knowing for production.)*

### Step 4 — Add the trigger (7 min)
1. **Overview** page → **Triggers** section → **Add trigger**.
2. Choose **When a new email arrives** (Outlook). Sign in when prompted (this uses your account).
   > No inbox to spare? Choose **Recurrence** instead (e.g., every 1 hour) — the rest of the lab is the same, minus the email content.
3. **Next** → keep the default payload (it hands the agent the email's subject and body) → **Finish**.

### Step 5 — Tell the agent what to do when it fires (4 min)
1. **Overview → Instructions → Edit**, add and **Save**:
   ```
   When an email arrives about an IT problem, use the Draft IT Ticket tool on the email body and show the resulting ticket. Ignore emails that aren't IT-related.
   ```
   *(You'll add a Teams post as a flow in Lab 4 — for now the agent just drafts the ticket.)*
2. **Publish** the agent — a trigger only runs after publishing.

### Step 6 — Fire it and watch (3 min)
1. Send yourself an email — subject `VPN down`, body `I can't connect to the VPN from home and need it for a 2pm meeting.`
2. Open the **Activity** page (left nav). Within a minute or two you'll see the trigger fire and the agent's reaction — the ticket it drafted — recorded step by step.
   > Triggers can take a moment. If nothing shows during class, open **Activity** and talk through it: the agent ran **with no one chatting to it.** That's an autonomous agent — impossible in Agent Builder.

---

## ⭐ Finished early? Optional stretch goals

### Stretch A — A second tool type: a connector (~6 min)
1. **Tools** → **+ Add a tool** → **Connector** → search a no-auth service (e.g., **MSN Weather → Get current weather**) → add it, name it clearly, **Add to agent**.
2. Instruction: `When asked about the weather, use the weather tool.` Test `what's the weather in Boise?` — a different tool type, same pattern: the agent picks it by description, fills the input, and acts.

### Stretch B — Make the trigger precise (~5 min)
1. Edit the trigger's instructions so it only acts on **urgent** emails (e.g., subject contains `urgent`) and does nothing otherwise.
2. Send one urgent and one normal email; confirm in **Activity** that only the urgent one triggered a reaction. Tight conditions are how autonomous agents stay useful instead of noisy.

---

## ✅ Done — and how to reuse it
Your agent can now **do** (a tool) and **act on its own** (a trigger) — the two capabilities that separate Copilot Studio from Agent Builder. **Reuse idea:** a Prompt tool is the fastest way to add a repeatable AI task (summarize, classify, draft); an event trigger turns any agent into a background worker that watches a mailbox, a list, or a schedule.

| Check | |
|---|---|
| **Draft IT Ticket** Prompt tool created with an input | ☐ |
| Tool added to the agent, and it **fired in the activity map** | ☐ |
| An **event trigger** added (email or recurrence) and the agent **published** | ☐ |
| Trigger fired and the reaction appears on the **Activity** page | ☐ |

*Next lab: build a **flow** — the richest tool type — and have the agent run it.*
