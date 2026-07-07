# Lab 1 — Create the HP Workplace Assistant

**Course:** PL-7008 — Create agents in Microsoft Copilot Studio
**Session:** 1
**Time:** ~30 minutes

> **Lab Scenario.** Across the labs you'll grow **one agent** — the **HP Workplace Assistant** — an internal helper that answers employee questions across IT, HR, and travel/expense, and takes real action. Each lab adds one capability: **create + ground** (here) → **a structured topic** (Lab 2) → **tools + a trigger** (Lab 3) → **a flow with logic** (Lab 4) → **connect a second agent** (optional Lab 5). Each lab also stands on its own, so if you fall behind you can start fresh at the top of any lab.
> **Why Copilot Studio and not Agent Builder?** This first step is deliberately simple — a grounded Q&A agent that, honestly, you *could* build in M365 Agent Builder. You're here for what comes next: a **structured intake topic** with entities and a branch (Lab 2), **tools and a trigger** so it acts on its own (Lab 3), **a flow with real logic** (Lab 4), and **connecting a second agent** (optional Lab 5). None of those are possible in Agent Builder. Even in this lab you'll use two Studio-only capabilities: the **activity map** that shows *how* the agent reached its answer, and a built-in **evaluation** that scores it for you.

## What you'll do
Create an agent from a plain-English prompt, give it a name and instructions, add one knowledge source, test it, publish it to a demo website, and run an evaluation to confirm it answers well.

## Before you start
- Sign in to Copilot Studio at **`https://copilotstudio.microsoft.com/`** — you'll be in **HP's default environment**. No setup needed.
- Download **`HP_IT_Support_FAQ.docx`** from the class files location to your computer.
- If several of you share the environment, **put your initials in front of the agent name** (e.g., `JS HP Workplace Assistant`) so they don't clash.
- Stay on the **current** Copilot Studio experience (not the preview) so the steps match.

---

## Your turn

### Step 1 — Create the agent
1. Go to `https://copilotstudio.microsoft.com/`. You'll be in **HP's default environment** (shown top-right) — nothing to set up.
2. Select **Agents** in the left navigation.
3. In the "describe your agent" box, paste this prompt and select **Send**:

   ```
   You are the HP Workplace Assistant. You help HP employees get quick, accurate answers to everyday workplace questions about IT support, HR and benefits, and travel and expenses. Be concise and friendly, and point people to the right team when a request needs a human.
   ```

4. Wait ~30–60 seconds for the agent to be created.

### Step 2 — Name and shape it
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

### Step 3 — Add a knowledge source
1. Select the **Knowledge** tab → **+ Add knowledge** → **Upload file**.
2. In the **Upload files** dialog, drag in **`HP_IT_Support_FAQ.docx`** (or **browse your device**).
3. Select **Add to agent**. *(A device upload has **no Name or Description fields** — the agent uses the **filename** as the label and semantically searches the file's **content**, so a clear filename like `HP_IT_Support_FAQ` is the routing hint. Want a description instead? Add the doc via **SharePoint** or a **website**, which have a Name + Description dialog.)*
4. The file shows **Indexing → Ready**. *Indexing can take a few minutes — keep going and test at the end.*

### Step 4 — Test it, and watch *how* it answers
1. Open the **Test** pane (top-right). Select the ellipsis (**…**) next to **{x}** and set **Show activity map** to **On**.
2. Try:
   ```
   How do I connect to the VPN?
   ```
   ```
   How do I request a new laptop?
   ```
3. **Read the activity map — this is the Copilot Studio difference.** For each answer it shows *what the agent did*: which knowledge source it searched (the **IT Support FAQ**) and that it grounded the reply on that file. Agent Builder just hands you an answer; Studio shows you the reasoning, so you can trust it — and debug it. *(If answers are empty, the file is still indexing — wait a minute and retry.)*
4. Now ask something outside the FAQ — e.g., `How much vacation do I get?` The activity map shows the **IT Support FAQ didn't match** (that's an HR question — you'd add an HR source the same way you added this one). Seeing *why* an answer is thin is the debugging view Agent Builder doesn't give you.

### Step 5 — Publish to a demo website
1. **Turn off authentication first** — the demo website won't open otherwise. Go to **Settings** (top bar) → **Security** → **Authentication** → select **No authentication** → **Save**. *(Fine for a class demo; production agents use real sign-in.)*
2. Select **Publish** → **Publish**. *(Auth changes only take effect after you publish.)*
3. Select the **Channels** tab → **Demo website**.
4. **Welcome message:** `Ask me about IT, HR, or travel and expenses.`
5. **Conversation starters:** add `How do I reset my password?` and `How do I request a new laptop?` — real buttons users can press instead of facing a blank box. → **Save**.
6. Select **Open demo website**, press a conversation starter, and confirm it answers.
> **Demo site still asks you to sign in or won't load?** Confirm **Authentication = No authentication** and that you **re-published** after changing it.

### Step 6 — Evaluate it
Don't judge an agent by one lucky question. Copilot Studio can test it *for* you — it generates questions from your knowledge and scores the answers automatically.
1. In your agent, open the **Evaluate** tab *(in some tenants this appears as **Analytics → Evaluation** — same feature)*.
2. Create a new evaluation / test set and choose the option to **generate test cases from knowledge** — Copilot Studio writes questions from the IT Support FAQ.
3. Skim the generated questions and delete any odd ones, then **Run** the evaluation.
4. Open the results: each test case gets **Pass** or **Fail**, and the set gets a pass rate. Open one case to see the agent's answer, the reasoning behind the score, and which knowledge source it used.

> **Why this matters:** production agents are tuned with evaluations, not gut feel. In 30 minutes you've built a working agent *and* the regression test you'll re-run after every future change to it.

---

## ⭐ Finished early? Optional stretch goals

*Pick one — both are self-contained and safe to skip.*

### Stretch A — Explore a template and compare orchestration styles
Templates are the fastest way to start a real agent — and this one shows you a second orchestration style.
1. **Agents** → under **Start with an agent template**, select **Safe Travels** → **Create**.
2. On **Overview**, read its instructions; on **Knowledge**, note it grounds on a *public website* — a different knowledge source type from the file you uploaded.
3. Open **Settings** and look at **Orchestration**: this template uses **classic** orchestration (topics decide everything), while your Workplace Assistant uses **generative** orchestration (the agent decides using instructions, knowledge, and tools). Close without changing anything.
4. **Topics** tab → **System** filter → open **Conversation Start** — this is where a welcome message actually lives in a classic agent.
5. In the **Test** pane, toggle **Track between topics** to **On**. Say `Hello`, then ask `What is Copilot Studio?` — watch the **Greeting** topic fire, then the **Fallback** topic catch the question it can't answer.
6. When done, delete the template agent (**Agents** list → **…** → **Delete**) so the shared environment stays clean.

### Stretch B — See it being used: analytics & feedback
Your agent quietly tracks how it's doing — with one catch worth knowing.
> **Why the Analytics tab looks empty:** it **ignores the Test pane.** Only real conversations on a **published channel** (the Demo website you just published) count, and they take about a day to aggregate. So this is *seed it now, read it tomorrow.*
1. Open your **Demo website** (**Channels → Demo website → Open**) and have 2–3 real conversations — ask a couple of IT questions, then click **thumbs up / thumbs down** on the answers. That thumbs signal is what feeds satisfaction.
2. In Copilot Studio, open the **Activity** tab *(**Monitor** in some tenants)* — your demo-website session shows up here **quickly**, with the full transcript and which knowledge source it used. This is the real-time view.
3. Open the **Analytics** tab and find where each number lives — sessions, conversation outcomes, generated-answer quality, knowledge-source use, satisfaction. **Expect zeros today**; the demo sessions and thumbs land tomorrow (quality scoring needs 10+ answers/day). Knowing the Test pane doesn't count — and where to actually look — is the point.

---

## ✅ Done — and how to reuse it
You created the HP Workplace Assistant, shaped its behavior, grounded it on a real document, published it, and evaluated it. **This is a working starting point** — back at your desk you can add your team's real documents and publish it to Teams for your group.

| Check | |
|---|---|
| Agent named **HP Workplace Assistant** with custom instructions | ☐ |
| **IT Support FAQ** added as knowledge (Ready) | ☐ |
| Answered an IT question in the **Test** pane | ☐ |
| Read the **activity map** to see *how* it grounded the answer | ☐ |
| Published to the **Demo website** | ☐ |
| Ran an **evaluation** and reviewed the pass rate | ☐ |

*Next lab: give the assistant a structured way to log an IT request.*
