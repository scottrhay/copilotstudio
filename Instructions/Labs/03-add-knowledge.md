# Lab 3 — Answer across multiple sources

**Course:** PL-7008 — Create agents in Microsoft Copilot Studio
**Session:** 2
**Time:** ~30 minutes (two short activities)

> **Lab Scenario — the core use case.** A real workplace assistant has to pull from **several documents at once** and synthesize one clear answer. You'll give the HP Workplace Assistant three knowledge sources across IT, HR, and travel/expense, tell it to answer only from them, and watch it search across all three. **No special setup — just uploaded files.**

## Quick start (so everyone can begin)
- **If you have your agent from Lab 1/2:** open **HP Workplace Assistant** → **Knowledge** tab.
- **If you're starting fresh:** create an agent named `HP Workplace Assistant` with the prompt below, then go to **Knowledge**:
  ```
  You are the HP Workplace Assistant that answers HP employee questions about IT, HR and benefits, and travel and expenses.
  ```
- Download these three files from this repo's **Files** folder first: [`HP_IT_Support_FAQ.docx`](../../Files/HP_IT_Support_FAQ.docx), [`HP_HR_Benefits_FAQ.docx`](../../Files/HP_HR_Benefits_FAQ.docx), [`HP_Travel_Expense_Policy.docx`](../../Files/HP_Travel_Expense_Policy.docx).

> **Tip:** uploaded files take a few minutes to index. Add them, keep working, and they'll be ready by the time you test. If anything looks unavailable, ask your instructor.

---

## Activity A — Add the knowledge sources (12 min)

For **each** of the three files: **Knowledge** tab → **+ Add knowledge** → **Upload file** → browse to the file → give it a clear **name** and **description** → **Add to agent**.

| File | Name | Description (helps the agent choose it) |
|---|---|---|
| `HP_IT_Support_FAQ.docx` | `IT Support FAQ` | `Passwords, MFA, VPN, devices, software, and logging IT tickets.` |
| `HP_HR_Benefits_FAQ.docx` | `HR & Benefits FAQ` | `PTO and leave, benefits enrollment, payroll, and remote work policy.` |
| `HP_Travel_Expense_Policy.docx` | `Travel & Expense Policy` | `What employees can claim, limits, per diem, and the expense workflow.` |

> Each file shows **Indexing**, then **Ready**. Indexing can take a few minutes — add all three, then move to Activity B while they finish. *(If you did Lab 1, the IT FAQ may already be here — don't add it twice.)*

---

## Activity B — Ground it and test the synthesis (18 min)

### Step 1 — Make it answer only from your sources (5 min)
1. Select **Settings** (top-right) → **Generative AI**.
2. In the **Knowledge** section, set **Allow ungrounded responses** to **Off** *(older tenants label this **Use general knowledge**)* — the agent must answer from your documents only.
3. Set **Use information from the Web** to **Off**.
4. Select **Save**, then close Settings.

### Step 2 — Confirm all three are Ready (2 min)
1. **Knowledge** tab → check that all three sources show **Ready**. If one is still **Indexing**, wait and refresh.

### Step 3 — Test across sources (11 min)
1. Open the **Test** pane → set **Show activity map** to **On** → **Start new test session** (**+**).
2. One source at a time — confirm each domain answers:
   ```
   How do I reset my password?
   ```
   ```
   How much PTO do I get?
   ```
   ```
   What can I claim for travel expenses?
   ```
   Watch the activity map: each answer is grounded in the matching document.

3. **The synthesis moment** — ask one question that spans sources:
   ```
   I'm a new hire this week. What do I need to sort out for IT and for HR?
   ```
   The assistant pulls from **both** the IT and HR FAQs in a single answer. That's "search across multiple sources and synthesize."

4. Ask something none of the sources cover:
   ```
   What's the company stock price today?
   ```
   Because general knowledge and web are off, it should say it doesn't know and suggest who to ask — exactly what you want for a grounded internal assistant.

---

## ⭐ Finished early? Optional stretch goals — if time permits

*Pick one. All are self-contained, optional, and safe to skip.*

### Stretch A — Evaluate, fix, re-run: the real tuning loop (~12 min)
1. Open the **Evaluate** tab *(in some tenants: **Analytics → Evaluation**)* → create a test set → **generate test cases from knowledge**. Check the questions cover all three sources; add one of your own per source if not. **Run** it.
2. Find a **Fail** (or the shakiest Pass) and read the scoring reasoning — it's usually a vague source description or a question the documents don't actually answer.
3. Make **one** fix — e.g., sharpen a knowledge source's description — then run the *same* test set again and use **Compare with** to see whether the pass rate moved.

> **Why this matters:** measure → change → re-measure is exactly how production agents get tuned. One variable at a time, always against the same test set.

### Stretch B — The ambiguity trap (~5 min)
1. Ask a question that could live in two sources:
   ```
   Can I expense my home internet?
   ```
   It plausibly belongs to the **Travel & Expense Policy** *or* the **HR & Benefits FAQ** (remote work).
2. Watch the **activity map**: which source(s) did it search? Is the answer grounded in one, both, or hedged?
3. Sharpen one description to claim that territory (e.g., add `home office and remote-work equipment costs` to the one that should own it), ask again, and see if the routing improves.

### Stretch C — Add a live website source (~8 min)
Files aren't the only grounding — an agent can also cite the public web, scoped to exactly the sites you choose.
1. **Knowledge** → **+ Add knowledge** → **Public websites** → enter `https://www.irs.gov/publications/p463` (official IRS travel-expense guidance — a natural companion to the internal travel policy) → **Add**.
2. **Name:** `IRS travel expense rules` · **Description:** `Official IRS guidance on deductible travel, per diem, and car expenses.` → **Add to agent**.
3. Once indexed, ask: `What does the IRS say about per diem rates?` — watch the activity map pick the *website* while `What can I claim for travel?` still hits your internal policy. Internal answers from internal docs, public facts from a source you chose.

> Website indexing takes a few minutes — kick it off, do Stretch A or B, then come back.

---

## ✅ Done — and how to reuse it
Your assistant now answers across **three** sources and stays grounded. **Reuse idea:** point it at your team's real policy and FAQ documents and you have a first-draft internal help agent. Keep spreadsheets to one sheet, and write a clear description for every source so the agent knows when to use it.

| Check | |
|---|---|
| Three knowledge sources added and **Ready** | ☐ |
| **Use general knowledge** and **Web** turned **Off** | ☐ |
| Each domain (IT / HR / Travel) answered from its document | ☐ |
| The "new hire" question pulled from **two** sources at once | ☐ |
| An out-of-scope question was politely declined | ☐ |

*Next lab: let the assistant take an action — post a request to Teams.*
