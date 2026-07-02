# Lab 2 — Add a "Log an IT Request" topic

**Course:** PL-7008 — Create agents in Microsoft Copilot Studio
**Session:** 1
**Time:** ~30 minutes

> **Lab Scenario.** Your HP Workplace Assistant can answer questions. Now you'll give it a **structured flow** so it can *collect* a request step by step — name, issue, and urgency — and confirm it back. Topics are how you make part of a conversation predictable while generative AI handles the rest.

## Quick start (so everyone can begin)
- **If you have your Lab 1 agent:** open **HP Workplace Assistant** and skip to Step 1.
- **If you're starting fresh:** Agents → create one with this prompt, name it `HP Workplace Assistant`, then continue:
  ```
  You are the HP Workplace Assistant that helps HP employees log and route IT support requests.
  ```
- Put your initials in front of names if you share an environment.

---

## Your turn

### Step 1 — Create the topic with Copilot (8 min)
1. Open your agent → **Topics** tab → **+ Add a topic** → **Add from description with Copilot**.
2. **Name your topic:** `Log IT Request`
3. **Create a topic to…:**
   ```
   Ask the employee for their name, a short description of their IT issue, and how urgent it is (High, Medium, or Low).
   ```
4. Select **Create**, then **Save**. Copilot drafts the question nodes for you.

### Step 2 — Polish a question with natural language (6 min)
1. If the **Edit with Copilot** panel isn't open on the right, select the **Copilot** icon above the canvas.
2. Select the **urgency** question node.
3. In **What do you want to do?**, enter:
   ```
   Make this a multiple choice question with three options: High, Medium, Low.
   ```
4. Select **Update**, then **Save**. *(If it doesn't look right, select Undo and try again — this is normal.)*

### Step 3 — Summarize with an Adaptive Card (7 min)
1. Click an empty area of the canvas so no node is selected.
2. In **Edit with Copilot**, enter:
   ```
   Add an adaptive card at the end that summarizes the name, issue, and urgency that were collected.
   ```
3. Select **Update**. A summary card is added. Select **Save**.

> If the card doesn't populate, open its **Media** box and confirm it references the topic variables (name, issue, urgency). You can re-run the prompt above if needed.

### Step 4 — Make the details reusable (4 min)
1. In the top bar, select **Variables** (or **More → Variables**).
2. Expand **Topic** variables and select the right-hand checkboxes for the **name**, **issue**, and **urgency** variables. This makes them available to other parts of the agent — the habit that lets topics and tools share details.
3. Select **Save**.

### Step 5 — Point the agent at the topic, then test (5 min)
1. **Overview** tab → **Instructions** → **Edit**. Type `When someone wants to report or log an IT problem, use the ` — then type **/** and pick **Log IT Request** from the picker, and finish with ` topic.` **Save**.
   > The **/** turns the topic name into a live reference — routing is more reliable than typing the name as plain text.
2. Open the **Test** pane → **Start new test session** (**+**) and enter:
   ```
   I need to log an IT issue
   ```
3. Provide a name, an issue ("my VPN won't connect"), and pick an urgency. Confirm the summary card shows what you entered.

---

## ⭐ Finished early? Optional stretch goals — if time permits

*Pick one. All are self-contained, optional, and safe to skip.*

### Stretch A — Branch on urgency (~10 min)
High-urgency requests should feel different from routine ones.
1. On the topic canvas, after the urgency question, select **+** → **Add a condition**.
2. Set the condition to your **urgency** variable **is equal to** `High`.
3. On the **High** branch, add a **Message** node: `This is marked HIGH — the IT hotline will call you back within the hour.`
   On the **other** branch: `Your request is logged — expect an update within one business day.`
4. **Save**, then test the topic twice — once choosing High, once Low — and confirm each path shows its own message.

### Stretch B — Watch slot filling do your work (~5 min)
Question nodes can pull answers out of what the user *already said*.
1. **Test** pane → **Start new test session** (**+**). Instead of the plain trigger, enter it all in one line:
   ```
   I need to log a HIGH priority IT issue - my VPN will not connect
   ```
2. Watch the flow: the urgency question should be **skipped** — the multiple-choice node picked `High` straight out of your message. Depending on entity settings, the issue may pre-fill too.
3. This is *slot filling*: the fewer questions a user has to re-answer, the better the intake feels. Note which questions still asked, and consider what entity types would let them pre-fill.

### Stretch C — Build a topic from blank and chain it (~12 min)
Copilot drafted your first topic. Now build one by hand and connect the two.
1. First, a cleanup pro move: **Topics** → **System** filter → toggle **Escalate** to **Off**. There's no human hand-off behind this agent, and fewer active topics means cleaner routing decisions.
2. **+ Add a topic** → **From blank**. Open **Details** (**More → Details** if hidden): **Name** `IT Issue Timing`; **Model description** `Use this topic to find out when an IT issue started before logging it`. Leave the trigger as **The agent chooses** — with generative orchestration, that description *is* the trigger.
3. Add a **Question** node: `When did the issue start?` → under **Identify**, select **Date and time** → name the variable `IssueStart`.
4. Below it, add **Topic management → Go to another topic → Log IT Request**. **Save**.
5. Test: `my laptop broke yesterday and I want to report it` — the timing topic collects the date (notice it understands "yesterday"), then hands off to your intake topic. That's topic chaining: small single-purpose topics composed into a flow.

---

## ✅ Done — and how to reuse it
You added a structured **Log IT Request** flow to the assistant. **Reuse idea:** the same pattern — collect a few fields, confirm with a card — works for any intake your team does (access requests, facilities tickets, equipment orders).

| Check | |
|---|---|
| **Log IT Request** topic created with name / issue / urgency | ☐ |
| Urgency is a **multiple-choice** question (High/Medium/Low) | ☐ |
| **Adaptive Card** summarizes the request | ☐ |
| Variables shared so other parts of the agent can use them | ☐ |
| Tested the flow end to end | ☐ |

*Next lab: let the assistant answer from several documents at once.*
