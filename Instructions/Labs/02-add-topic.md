# Lab 2 — Add a "Log an IT Request" topic

**Course:** PL-7008 — Create agents in Microsoft Copilot Studio
**Session:** 1
**Time:** ~35 minutes

> **Lab Scenario.** A topic is where you make part of the conversation *predictable* — pure Copilot Studio, not Agent Builder. You'll build a **Log IT Request** intake that's more than a list of questions: it uses **entities** to control what the agent accepts (a reusable closed-list for urgency, the built-in Date entity for timing), stores answers in **variables**, and **branches** on those variables so a High-urgency request is handled differently. Generative AI still handles everything outside the topic.

## Quick start (so everyone can begin)
- **If you have your Lab 1 agent:** open **HP Workplace Assistant** and skip to Step 1.
- **If you're starting fresh:** Agents → create one with this prompt, name it `HP Workplace Assistant`, then continue:
  ```
  You are the HP Workplace Assistant that helps HP employees log and route IT support requests.
  ```
- Put your initials in front of names if you share an environment.

---

## Your turn

### Step 1 — Create the topic with Copilot (6 min)
1. Open your agent → **Topics** tab → **+ Add a topic** → **Add from description with Copilot**.
2. **Name your topic:** `Log IT Request`
3. **Create a topic to…:**
   ```
   Ask the employee for their name, a short description of their IT issue, and how urgent it is (High, Medium, or Low).
   ```
4. Select **Create**, then **Save**. Copilot drafts the question nodes for you.

### Step 2 — Check the questions, and edit with natural language (6 min)
1. Look at what Copilot built from your description — three **Question** nodes: name, issue, and urgency. Because your Step 1 prompt named the choices ("High, Medium, or Low"), Copilot already made **urgency a multiple-choice question** — you don't have to convert it. *(Describing the options in the prompt is what does it.)*
2. Now practice editing with plain English — the real Copilot Studio skill. On the topic toolbar, select **Copilot** to open the **Edit with Copilot** panel.
3. Describe a change, then select **Update**. Try:
   ```
   Add a message at the very start that says: I'll help you log your IT request — this takes about a minute.
   ```
   Copilot adds the message node for you — no need to say "add a message node." Use **Undo** if you don't like it, then **Save**.
   > **Heads-up — Copilot usually drops the new node at the *end* of the topic, not the start.** "At the very start" is a hint, not a guarantee. If it lands in the wrong place, move it by hand: select the message node → click the **cut** (scissors) icon on its toolbar → scroll to the **top** of the canvas → click the **+** just under the trigger → choose **Paste** from the node menu → **Save**. Repositioning a node this way is a normal Copilot Studio skill — worth doing once so you know how.

> **If urgency came out as free text** (not the three buttons): either tell Copilot `Make the urgency question multiple choice with options High, Medium, and Low`, or do it by hand — select the **urgency** question node, set **Identify** to **Multiple choice options**, and add **High**, **Medium**, and **Low** under **Options for user**. **Save**.

### Step 3 — Configure entities: control what the agent accepts (7 min)
Entities decide *what counts as a valid answer.* You'll use both kinds.
1. **A prebuilt entity.** In **Edit with Copilot**, enter:
   ```
   Add a question that asks when the issue started and accepts a date.
   ```
   **Update.** Copilot adds a **Question** node that uses the built-in **Date and time** entity — open the new node, confirm **Identify** shows *Date and time*, and name its variable `IssueStart`. **Save**.
2. **A reusable closed-list entity.** Your urgency options are inline today; make them a **reusable entity** so any topic can validate urgency the same way. Select the **urgency** question node → **Identify** → **Create a new entity** → **Closed list** → name it `IT Urgency`, add items **High**, **Medium**, **Low** → **Save**. *(Custom entities live once at agent level under **Settings → Entities**.)*

### Step 4 — Use a variable to branch the flow (6 min)
Variables hold what you collected; a **Condition** lets the agent act on them.
1. On the canvas, after the urgency question, select **+** → **Add a condition**.
2. Set it: your **urgency** variable **is equal to** `High`.
3. On the **High** branch, add a **Message**: `This is marked HIGH — the IT hotline will call you back within the hour.`
   On the **All other conditions** branch, add: `Your request is logged — expect an update within one business day.`
4. **Save.** You'll test both paths in Step 7.

### Step 5 — Summarize with an Adaptive Card (5 min)
1. Click an empty area of the canvas so no node is selected.
2. In **Edit with Copilot**, enter:
   ```
   Add an adaptive card at the end that summarizes the name, issue, date started, and urgency.
   ```
3. Select **Update**. A summary card is added. Select **Save**.

> If the card doesn't populate, open its **Media** box and confirm it references the topic variables (name, issue, urgency). You can re-run the prompt above if needed.

### Step 6 — Make the details reusable (3 min)
1. In the top bar, select **Variables** (or **More → Variables**).
2. Expand **Topic** variables and tick the right-hand checkboxes for **name**, **issue**, **IssueStart**, and **urgency** — this shares them with the rest of the agent (you'll reuse **urgency** in Lab 4). **Save**.

### Step 7 — Point the agent at the topic, then test both paths (5 min)
1. **Overview** tab → **Instructions** → **Edit**. Type `When someone wants to report or log an IT problem, use the ` — then type **/** and pick **Log IT Request** from the picker, and finish with ` topic.` **Save**.
   > The **/** turns the topic name into a live reference — more reliable than typing the name as plain text.
2. Open the **Test** pane → **Start new test session** (**+**) → `I need to log an IT issue`. Provide a name, an issue ("my VPN won't connect"), a start date, and pick **High** — confirm the **HIGH** message *and* the summary card.
3. Start another session and pick **Low** — confirm the routine message instead. Two paths from one topic: that's **entities → variables → conditions** at work.

---

## ⭐ Finished early? Optional stretch goals — if time permits

*Pick one. All are self-contained, optional, and safe to skip.*

### Stretch A — Make slot filling actually work (~7 min)
Slot filling lets the agent pull answers out of what the user *already said* — but **only from questions that use an entity.** A plain-text question has nothing to extract from, so it always asks. That's the catch most people hit: they type everything in one line and the agent still asks for their name. Here's how to fix it and see it work.

1. **The reason it doesn't "just work":** your **name** and **issue** questions are free text (Copilot drafted them that way), so there's nothing for the agent to extract. Only **urgency** uses an entity, so only urgency can pre-fill.
2. **Give the name question an entity.** Open the **name** question node → **Identify** → choose the prebuilt **Person name** entity *(labeled "Person name" or "Name" depending on tenant)*. **Save**. Now the agent knows how to spot a name in a sentence.
3. **Test it.** **Test** pane → **Start new test session** (**+**) → enter it all in one line:
   ```
   this is Scott and I need to log a HIGH priority IT issue
   ```
4. **What you should see:** the agent **skips the name question** (it extracted `Scott`) **and the urgency question** (it extracted `High`), and asks only for the **issue** — because that one is still free text with no entity. That contrast *is* the lesson: entity-backed questions slot-fill; free-text ones don't.
5. **See it happen:** open the **Variables** panel → **Test** tab and watch `name` and `urgency` fill in before those questions would have run. To force a question to always ask even when it could pre-fill, open its node → **⋯ → Properties → Question behavior → Ask every time**.

> **Reality check:** slot filling is best-effort natural-language extraction, not a guarantee — a messy sentence may still ask. Under generative orchestration the agent also fills inputs on its own. The dependable rule to teach: *if you want a field to pre-fill, back it with an entity.*

### Stretch B — Build a topic from blank and chain it (~12 min)
Copilot drafted your first topic. Now build one by hand and connect the two.
1. First, a cleanup pro move: **Topics** → **System** filter → toggle **Escalate** to **Off**. There's no human hand-off behind this agent, and fewer active topics means cleaner routing decisions.
2. **+ Add a topic** → **From blank**. Open **Details** (**More → Details** if hidden): **Name** `IT Issue Timing`; **Model description** `Use this topic to find out when an IT issue started before logging it`. Leave the trigger as **The agent chooses** — with generative orchestration, that description *is* the trigger.
3. Add a **Question** node: `When did the issue start?` → under **Identify**, select **Date and time** → name the variable `IssueStart`. **Save.**
4. Below it, add **Topic management → Go to another topic → Log IT Request**. **Save**.
5. Test: `my laptop broke yesterday and I want to report it` — the timing topic collects the date (notice it understands "yesterday"), then hands off to your intake topic. That's topic chaining: small single-purpose topics composed into a flow.

> **If nothing happens / it feels empty:** generative orchestration only routes here if the model description clearly matches. Make the description more specific (e.g., add "when a user mentions an IT problem *without* saying when it started"), **Publish**, and retest — or add a **trigger phrase** like `when did my issue start` to force the route while you test. An unrouted topic looks "empty" because the agent never entered it.

### Stretch C — Generate a ticket number with Power Fx (~8 min)
The real power-user skill: compute a value instead of asking for it. You'll stamp each request with a unique ticket ID using **Power Fx**, Copilot Studio's formula language.
1. On the canvas, right after the last question (before the summary card), add a **Variable management → Set a variable value** node.
2. **Set variable:** create a new one named `TicketId`.
3. **To value:** switch the field to a **formula** (the **fx** toggle) and enter:
   ```
   "IT-" & Text(Now(), "yyyymmdd-hhmmss")
   ```
   This builds an ID like `IT-20260705-143210` from the current date and time. **Save.**
4. Add `TicketId` to your **Adaptive Card** (or a message before it) so the user sees their ticket number: *"Your request is logged as {TicketId}."*
5. Test the topic and confirm a unique ID appears. **Why it matters:** Power Fx lets a topic transform, format, and calculate — the same skill you'll use to shape tool inputs in Labs 3 and 4. Try another: `Upper(Trim(Topic.name))` to normalize the name, or `"Priority: " & Topic.urgency`.

---

## ✅ Done — and how to reuse it
You added a structured **Log IT Request** flow to the assistant. **Reuse idea:** the same pattern — collect a few fields, confirm with a card — works for any intake your team does (access requests, facilities tickets, equipment orders).

| Check | |
|---|---|
| **Log IT Request** topic collects name, issue, start date, urgency | ☐ |
| Urgency uses a reusable **closed-list entity** (`IT Urgency`); start date uses the **Date** entity | ☐ |
| A **Condition** branches on the urgency variable (High vs. all other) | ☐ |
| **Adaptive Card** summarizes the request | ☐ |
| Variables shared, and **both branches** tested | ☐ |

*Next lab: give the assistant **tools** and a **trigger** so it can act, not just talk.*
