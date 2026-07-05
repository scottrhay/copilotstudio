# Lab 5 (optional) — Connect a second agent

**Course:** PL-7008 — Create agents in Microsoft Copilot Studio
**Session:** 2
**Time:** ~25 minutes — **optional, only if time permits**

> **Lab Scenario.** One agent doesn't have to do everything. In real deployments you build **specialists** and let a **front-door agent route to them** — the orchestrator reads each connected agent's description and hands off the questions it's best at. Your **HP Workplace Assistant** becomes the front door; you'll build an **HR Benefits Specialist**, connect it, and watch a question get delegated. Multi-agent orchestration is **pure Copilot Studio — Agent Builder has nothing like it.**

## Quick start
- You need your **HP Workplace Assistant** from Labs 1–4, **published.** Its generative orchestration must be on (default) — connected agents require it.
- You'll create and publish a **second** agent in Part A, then connect it in Part B.
- Download **`HP_HR_Benefits_FAQ.docx`** from the class files location.

> **Why two agents instead of just adding HR knowledge to one?** Small agents with tight instructions are easier to trust, test, and hand to different owners — HR maintains theirs, IT maintains yours. Routing keeps each one focused. That's the production pattern this lab teaches.

---

## Part A — Build the specialist

### Step 1 — Create the HR Benefits Specialist
1. **Agents** → describe-your-agent box → paste, then **Send**:
   ```
   You are the HP HR Benefits Specialist. You answer employee questions about health benefits, PTO, retirement, and open enrollment, using only your configured HR knowledge. Be concise and point people to HR for anything you can't answer.
   ```
2. **Overview → Details → Edit:** **Name** `HR Benefits Specialist` *(add your initials if you share the environment)*; **Description** `Answers HR benefits, PTO, retirement, and open-enrollment questions.` → **Save**.

### Step 2 — Ground it and publish
1. **Knowledge** tab → **+ Add knowledge** → **Upload file** → **`HP_HR_Benefits_FAQ.docx`**. In the upload dialog give it **Name** `HR Benefits FAQ` and **Description** `Health benefits, PTO, retirement, and open enrollment.` → **Add to agent**. Wait for **Ready**.
2. **Publish** → **Publish**. *A connected agent must be published before another agent can route to it.*
3. Quick check in the **Test** pane: ask `How much PTO do new hires get?` and confirm it answers from the FAQ.

---

## Part B — Connect it to the front-door agent

### Step 3 — Add the connected agent
1. **Agents** → open your **HP Workplace Assistant** (the front door).
2. Go to the **Agents** area for this agent — the **Connected agents** section *(on the **Overview**/**Build** surface, look for **Agents** or **Connected agents**; the exact label varies by tenant)*.
3. Select **+ Add** → **Connected agent** → pick **HR Benefits Specialist** from the list of published agents.
4. In the **description of when to use it**, write — this is what the orchestrator reads to route:
   ```
   Use the HR Benefits Specialist for any question about health benefits, PTO, vacation, retirement, or open enrollment. Do not use it for IT problems.
   ```
5. **Save**.

### Step 4 — Tell the front-door agent it can delegate
1. **Overview → Instructions → Edit**, add and **Save**:
   ```
   You can answer IT questions yourself. For HR benefits, PTO, retirement, or open enrollment, hand the question to the HR Benefits Specialist.
   ```
2. **Publish** the HP Workplace Assistant — routing only takes effect after publishing.

---

## Part C — Watch the hand-off
1. In the HP Workplace Assistant, open the **Test** pane → set **Show activity map** to **On** → **Start new test session** (**+**).
2. Ask an **IT** question: `How do I connect to the VPN?` — it answers **itself** from the IT FAQ (the activity map shows local knowledge).
3. Ask an **HR** question: `How many vacation days do I get?` — in the activity map, watch it **route to the HR Benefits Specialist**, which answers from *its* knowledge. One conversation, two agents.
4. Ask something neither owns: `What's the weather?` — confirm it declines gracefully instead of mis-routing. Tight routing descriptions are what keep multi-agent systems predictable.

> Routing can take a few seconds, and the connected-agent node may be labeled with the specialist's name. If a question routes the wrong way, tighten the **description of when to use it** in Step 3 — that text, not the instructions, is the primary routing signal.

---

## ⭐ Finished early? Optional stretch goals

### Stretch A — Add a third specialist
Repeat Parts A–B with a **Travel & Expense Specialist** grounded on **`HP_Travel_Expense_Policy.docx`**. Now the front door routes across three domains. Notice the front-door agent's own knowledge can shrink — its job is increasingly to *route*, not to answer.

### Stretch B — Break the routing on purpose
Edit the HR specialist's **description of when to use it** to be vague (e.g., `Handles employee questions`). Re-test the VPN question and watch it sometimes mis-route to HR. Restore the specific description. This shows why the routing description is the single most important field in a multi-agent build.

---

## ✅ Done — and how to reuse it
You built a **multi-agent system**: a front-door agent that routes to specialists it can trust. **Reuse idea:** give each team its own agent (IT, HR, Facilities), then put one front-door assistant in front of all of them. Teams own their content; employees see one place to ask. This is how Copilot Studio scales past a single agent — and it's the capability Agent Builder can't touch.

| Check | |
|---|---|
| **HR Benefits Specialist** agent created, grounded, and **published** | ☐ |
| It answered an HR question from its own knowledge | ☐ |
| Connected to the **HP Workplace Assistant** with a routing description | ☐ |
| Front-door agent **published** after connecting | ☐ |
| In the activity map, an HR question **routed to the specialist**; an IT question stayed local | ☐ |

*This is the end of the lab track: one agent that grounds, runs a topic, uses tools and triggers, calls a flow, and delegates to other agents.*
