# Lab 1 — Create the HP Workplace Assistant

**Course:** PL-7008 — Create agents in Microsoft Copilot Studio
**Session:** 1
**Time:** ~30 minutes

> **Lab Scenario.** Across the labs you'll grow **one agent** — the **HP Workplace Assistant** — an internal helper that answers employee questions across IT, HR, and travel/expense, and takes real action. Each lab adds one capability: **create + ground** (here) → **a structured topic** (Lab 2) → **tools + a trigger** (Lab 3) → **a flow with logic** (Lab 4) → **connect a second agent** (optional Lab 5). Each lab also stands on its own, so if you fall behind you can start fresh at the top of any lab.
> **Why Copilot Studio and not Agent Builder?** This first step is deliberately simple — a grounded Q&A agent that, honestly, you *could* build in M365 Agent Builder. You're here for what comes next: a **structured intake topic** with entities and a branch (Lab 2), **tools and a trigger** so it acts on its own (Lab 3), **a flow with real logic** (Lab 4), and **connecting a second agent** (optional Lab 5). None of those are possible in Agent Builder. Even in this lab you'll use one Studio-only capability: the **activity map** in Step 4, which shows you *how* the agent reached its answer.

## What you'll do
Create an agent from a plain-English prompt, give it a name and instructions, add one knowledge source, test it, and publish it to a demo website.

## Before you start
- Sign in to Copilot Studio at **`https://copilotstudio.microsoft.com/`** — you'll be in **HP's default environment**. No setup needed.
- Download **`HP_IT_Support_FAQ.docx`** from the class files location to your computer.
- If several of you share the environment, **put your initials in front of the agent name** (e.g., `JS HP Workplace Assistant`) so they don't clash.
- Stay on the **current** Copilot Studio experience (not the preview) so the steps match.

---

## Your turn

### Step 1 — Create the agent (5 min)
1. Go to `https://copilotstudio.microsoft.com/`. You'll be in **HP's default environment** (shown top-right) — nothing to set up.
2. Select **Agents** in the left navigation.
3. In the "describe your agent" box, paste this prompt and select **Send**:

   ```
   You are the HP Workplace Assistant. You help HP employees get quick, accurate answers to everyday workplace questions about IT support, HR and benefits, and travel and expenses. Be concise and friendly, and point people to the right team when a request needs a human.
   ```

4. Wait ~30–60 seconds for the agent to be created.

### Step 2 — Name and shape it (7 min)
1. On the **Overview** tab, in **Details** → **Edit**:
   - **Name:** `HP Workplace Assistant` *(add your initials in front if you share the environment)*
   - **Description:** `Helps HP employees with IT, HR, and travel/expense questions.`
   - Select **Save**.
2. In **Instructions** → **Edit**, add these lines, then **Save**:

   ```
   - Answer only from your configured knowledge. If you don't know, say so and suggest who to contact.
   - Do not give legal, medical, or financial advice.
   - Keep answers short. Use bullet points for lists.
   ```

3. In **Suggested prompts**, select **Add suggested prompts** and add one:
   - **Title:** `Reset password` — **Prompt:** `How do I reset my password?` — **Save**.

### Step 3 — Add a knowledge source (6 min)
1. Select the **Knowledge** tab → **+ Add knowledge** → **Upload file**.
2. Browse to **`HP_IT_Support_FAQ.docx`** (drag-and-drop, or browse to it).
3. **In the upload dialog, before you add it,** give it a clear **Name** and a **detailed Description** — the description is what the agent reads to decide when to use this source, so be specific:
   - **Name:** `IT Support FAQ`
   - **Description:** `Answers about passwords, MFA, VPN, devices, software, and how to log IT tickets.`
4. Select **Add to agent**. *(The description isn't shown on the Knowledge list afterward — to change it later, use the source's **⋮ → Edit**.)*
5. The file shows **Indexing** → **Ready**. *Indexing can take a few minutes — keep going and test at the end.*

### Step 4 — Test it, and watch *how* it answers (5 min)
1. Open the **Test** pane (top-right). Select the ellipsis (**…**) next to **{x}** and set **Show activity map** to **On**.
2. Try:
   ```
   How do I connect to the VPN?
   ```
   ```
   How do I request a new laptop?
   ```
3. **Read the activity map — this is the Copilot Studio difference.** For each answer it shows *what the agent did*: which knowledge source it searched (the **IT Support FAQ**) and that it grounded the reply on that file. Agent Builder just hands you an answer; Studio shows you the reasoning, so you can trust it — and debug it. *(If answers are empty, the file is still indexing — wait a minute and retry.)*
4. Now ask something outside the FAQ — e.g., `How much vacation do I get?` The activity map shows the **IT Support FAQ didn't match** (that's an HR question — you add HR in Lab 3). Seeing *why* an answer is thin is the debugging view Agent Builder doesn't give you.

### Step 5 — Publish to a demo website (5 min)
1. Select **Publish** → **Publish**.
2. Select the **Channels** tab → **Demo website**.
3. **Welcome message:** `Ask me about IT, HR, or travel and expenses.`
4. **Conversation starters:** add `How do I reset my password?` and `How do I request a new laptop?` — real buttons users can press instead of facing a blank box. → **Save**.
5. Select **Open demo website**, press a conversation starter, and confirm it answers.

---

## ⭐ Finished early? Optional stretch goals — if time permits

*Pick one. All are self-contained, optional, and safe to skip.*

### Stretch A — Shape the personality (~5 min)
1. **Overview** → **Instructions** → **Edit**. Add these lines, then **Save**:
   ```
   - End every answer by asking if there is anything else you can help with.
   - If a question is about an urgent IT outage, tell the user to call the IT hotline first, then answer.
   ```
2. In the **Test** pane, re-ask `How do I connect to the VPN?` and confirm both behaviors show up.
3. Add two more **Suggested prompts** (e.g., `Request software`, `Order a laptop`) so the demo website offers real starting points.

### Stretch B — Run your first evaluation (~10 min)
Copilot Studio can test your agent *for* you: it generates test questions from your knowledge and scores the answers automatically.
1. In your agent, open the **Evaluate** tab *(in some tenants this appears as **Analytics → Evaluation** — same feature)*.
2. Create a new evaluation / test set and choose the option to **generate test cases from knowledge** — Copilot Studio writes questions from the IT Support FAQ.
3. Skim the generated questions and delete any odd ones, then **Run** the evaluation.
4. Open the results: each test case gets **Pass** or **Fail**, and the set gets a pass rate. Open one case to see the agent's answer, the reasoning behind the score, and which knowledge source it used.

> **Why this matters:** production agents are tuned with evaluations, not gut feel. You just built a regression test you can re-run after every change you make to this agent.

### Stretch C — Explore a template and compare orchestration styles (~8 min)
Templates are the fastest way to start a real agent — and this one shows you a second orchestration style.
1. **Agents** → under **Start with an agent template**, select **Safe Travels** → **Create**.
2. On **Overview**, read its instructions; on **Knowledge**, note it grounds on a *public website* (you'll add one of those in Lab 3's stretch).
3. Open **Settings** and look at **Orchestration**: this temp