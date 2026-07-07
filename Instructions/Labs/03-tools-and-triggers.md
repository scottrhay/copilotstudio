# Lab 3 — Give the agent tools and a trigger

**Course:** PL-7008 — Create agents in Microsoft Copilot Studio
**Session:** 2
**Time:** ~30 minutes (two parts)

> **Lab Scenario.** Topics made the agent *predictable*; now you'll make it *capable* and *proactive* — two things **Agent Builder can't do at all.** A **tool** lets the agent *do* something: here, an AI **Prompt tool** that turns a messy problem into a clean IT ticket. A **trigger** lets the agent act **without being asked** — you'll wire an event trigger so it reacts on its own. This is the heart of what Copilot Studio adds.

## Quick start
- Open your **HP Workplace Assistant** (from Labs 1–2). Any agent with **generative orchestration** on works — triggers require it, and it's the default.
- No files to download. Two parts: **A — add a tool**, **B — add a trigger.**

---

## Part A — Add a tool: make the agent *do* something

A **tool** is how an agent acts. There are several kinds — connectors, workflows, computer use, REST, MCP, and **prompts**. You'll build a **Prompt tool**: pure AI, no external connection or sign-in, so it works in any tenant.

### Step 1 — Create the Prompt tool
1. Open your **HP Workplace Assistant** → **Tools** tab → **+ Add a tool** → **+ New tool** → **Prompt**. *(Build it from inside the agent — that lets you configure it inline in Step 2. If you instead use the left-nav **Tools** area, you'll finish with an **Add to an Agent** dialog and then have to reopen the tool from the agent to configure it.)*
2. **Name:** `Draft IT Ticket` — select the auto-generated name at the top-left to rename it.
3. In the instructions box, paste this. The blank **Ticket request:** line is where the user's text gets inserted in the next step:
   ```
   You turn the user's rough description of an IT problem into a clean support ticket.

   Ticket request: 

   Output exactly:
   - Title: a one-line summary
   - Category: one of Access, Hardware, Software, Network, Other
   - Summary: two sentences a technician can act on
   ```
4. **Insert the input** where the request goes: click at the end of the **Ticket request:** line, then type **`/`** (or select **Add content**) → under **Input**, choose **Text**. An input *pill* drops in at your cursor. Rename it from **Text input** to `IssueText`. *(You don't highlight and replace words — `/` inserts the token at the cursor.)*
5. Select the **IssueText** pill and add **Sample data** so you can test: `my laptop won't join the wifi and I have a demo in an hour`.
6. Select **Test** and confirm the response returns **Title / Category / Summary**. When satisfied, select **Save**.
7. Select **Add and configure** — the tool attaches to your agent and its configuration pane opens (continue in Step 2). *(No separate "Publish" step. If you built the tool from the left-nav **Tools** area instead, you'll get an **Add to an Agent** dialog — pick **HP Workplace Assistant** → **Add**, then reopen the tool from the agent's **Tools** tab to configure it.)*

### Step 2 — Configure the input and tell the agent when to use it
1. The tool's configuration pane opens. Under **Inputs**, `IssueText` is set to **Fill with AI** — the agent fills it automatically. Leave that selected.
2. Expand **Additional details** and add a **Description** (the pane nudges you: *"Provide a description to improve dynamic slot filling"* — a recommendation, not a hard stop, and it helps the agent fill the input reliably): `The user's plain-language description of their IT problem.` Leave **Identify as: User's entire response**.
3. Scroll to **Completion → After running** and select **Write the response with generative AI** — this makes the agent actually show the drafted ticket to the user. *(The default **Don't respond** can leave the agent silent after the tool runs.)* **Save**.
4. **Overview → Instructions → Edit**. Put this line in your **Step-by-Step Instructions** section — under the step where you handle an IT request — **not** under Skills. (Skills describes what the agent knows; *when to use a tool* is procedure.) Where the tool name goes, type **`/`** and pick **Draft IT Ticket** so it inserts a proper reference, not plain text. Then **Save**:
   ```
   When a user describes an IT problem in their own words, use the Draft IT Ticket tool to produce a clean ticket and show it to them.
   ```

> **Topic vs. tool — who does what.** You now have two IT paths, and the orchestrator needs to know which is which. The **Log IT Request topic** (Lab 2) is *structured intake* — it walks the user through name, issue, and urgency step by step. The **Draft IT Ticket tool** (this lab) *drafts a ticket from a freeform description*. They can coexist, but if both just trigger on "IT problem" the pick feels random — so tell the agent when to use each — in that same **Step-by-Step Instructions** section. For example: *"Use Draft IT Ticket when the user simply describes the problem; use the Log IT Request topic when they want to formally log or submit it."* Reference both the tool and the topic with **`/`** so the links are explicit.

### Step 3 — Test the tool, and watch it fire
1. **Test** pane → **Show activity map** On → **Start new test session** (**+**).
2. Enter a messy description:
   ```
   outlook keeps asking me to sign in over and over since this morning
   ```
3. The agent calls **Draft IT Ticket** and returns a structured ticket. **In the activity map, watch the tool node fire** — the agent *chose* the tool from its description. That decide-then-do is the tool pattern; Lab 4 builds the richest tool type, a **flow**.

---

## Part B — Add a trigger: make the agent act on its own

A **topic trigger** waits for a user to type. An **event trigger** fires on something happening in another system — an email arriving, a file created, a schedule — so the agent runs **autonomously.** *(Event triggers need generative orchestration (on by default), run on **your** credentials, and can affect billing — fine for this lab, worth knowing for production.)*

### Step 4 — Add the trigger
1. **Overview** page → **Triggers** section → **Add trigger**.
2. Choose **When a new email arrives (V3)** on the **Office 365 Outlook** connector. Sign in when prompted (this uses your account). *(The "(V3)" is just the connector version — pick that one.)*
   > No inbox to spare? Choose **Recurrence** instead (e.g., every 1 hour) — the rest of the lab is the same, minus the email content.
3. **Scope it so it doesn't fire on every email** — each match is billable. Set **Folder** = **Inbox** and **Subject Filter** = `IT:`; leave the rest blank. Now the agent only wakes for mail whose subject contains `IT:`.
4. **Next** → keep the default payload (it hands the agent the email's subject and body) → **Finish**.

### Step 5 — Tell the agent what to do when it fires
1. **Overview → Instructions → Edit**, add and **Save**:
   ```
   When an email arrives about an IT problem, use the Draft IT Ticket tool on the email body and show the resulting ticket. Ignore emails that aren't IT-related.
   ```
   *(You'll add a Teams post as a flow in Lab 4 — for now the agent just drafts the ticket.)*
2. **Publish** the agent — a trigger only runs after publishing.

### Step 6 — Fire it and watch
1. Send yourself an email — subject `IT: VPN down` (it must contain your `IT:` filter token), body `I can't connect to the VPN from home and need it for a 2pm meeting.`
2. Open the **Activity** page (left nav). Within a minute or two you'll see the trigger fire and the agent's reaction — the ticket it drafted — recorded step by step.
   > Triggers can take a moment. If nothing shows during class, open **Activity** and talk through it: the agent ran **with no one chatting to it.** That's an autonomous agent — impossible in Agent Builder.

---

## ⭐ Finished early? Optional stretch goals

### Stretch A — A second tool type: a connector
1. **Tools** → **+ Add a tool** → **Connector** → search a no-auth service (e.g., **MSN Weather → Get current weather**) → add it, name it clearly, **Add to agent**.
2. Instruction: `When asked about the weather, use the weather tool.` Test `what's the weather in Boise?` — a different tool type, same pattern: the agent picks it by description, fills the input, and acts.

### Stretch B — Make the trigger precise
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
