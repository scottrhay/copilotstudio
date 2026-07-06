# Copilot Studio — Student Quick Reference (HP)

**PL-7008 · Create agents in Microsoft Copilot Studio · Two-session HP delivery**

Today you build one agent — the **HP Workplace Assistant** — across the labs. Keep this beside you.

---

## 1. Microsoft 365 Copilot vs. Agents vs. Power Automate

|  | **Microsoft 365 Copilot** | **Agent (Copilot Studio)** | **Power Automate** |
|---|---|---|---|
| **What it is** | The AI assistant in your apps | A purpose-built assistant you create | Deterministic, rules-based automation |
| **You interact by** | Chatting in Word/Outlook/Teams | Chatting with the agent | Nothing — a trigger or call starts it |
| **Who builds it** | No one — it's ready | You (a maker) | You (an automation author) |
| **Driven by** | Your M365 data (Graph) | Instructions, knowledge, topics, tools | Connectors, conditions, actions |
| **HP example** | "Summarize this RFP thread" | "HP Workplace Assistant" | "Post meeting tasks to a Teams channel" |

**Rule of thumb:** *Human chatting → think **agent**. System acting → think **flow**.*
**Scope note:** this course builds **agents**. Power Automate is *out of scope* (it's the contrast case — deterministic, no conversation). The flows you build in Lab 4 are **Copilot Studio agent flows**, authored right inside the agent — not Power Automate.

---

## 2. Anatomy of a Copilot Studio agent

- **Instructions** — plain-language guidance for how the agent behaves. *Shapes* behavior; doesn't hard-enforce it.
- **Knowledge** — what it knows: uploaded files (Word/Excel/PDF), SharePoint, OneDrive, public websites. *(At HP: just upload your files — no database to set up.)*
- **Topics** — structured conversation flows for predictable, step-by-step interactions (collect details, confirm, branch).
- **Tools** — actions the agent takes in other systems via **workflows** and connectors (e.g., post to Teams).

**Generative orchestration ON** = the agent decides which knowledge/topics/tools to use. When it's on, your **descriptions** matter — write them clearly.

---

## 3. What you build, lab by lab

| Lab | You add | Skill |
|---|---|---|
| 1 | Create the assistant + instructions + first knowledge source; publish | Create & ground an agent |
| 2 | A **Log IT Request** topic — entities, variables, and a branch | Structured, validated intake |
| 3 | **Tools + triggers** — a Prompt tool that drafts a ticket, plus an event trigger | Make the agent *act*, and act on its own |
| 4 | **A multi-step flow** — the agent drafts a ticket, then a flow logs it to Excel, emails the requester, and alerts the IT lead if urgent | Multi-step logic in a tool |
| 5 *(optional)* | **Connected agents** — build an HR specialist and route to it | Multi-agent orchestration |
| 4B *(optional)* | **Turn a meeting into tasks** — same flow skills, posts to Teams | Multi-step logic in a tool |

*Answering across IT/HR/Travel sources is taught as a demo with an optional short practice — not a numbered lab.*

---

## 4. "What do I use when?" cheat sheet

- Answer from **my own documents/email**, now → **Microsoft 365 Copilot** (no build).
- A **reusable assistant** for my team → **build an agent**.
- A **predictable, step-by-step** intake → **a topic** inside the agent.
- The **same action every time**, no chat → **a workflow** added as a **tool**.
- Ground on your own data → **upload files** (one sheet for Excel) or point at a **website**.

---

## 5. Copy-ready prompts (used in the labs)

**Create the agent (Lab 1):**
```
You are the HP Workplace Assistant. You help HP employees get quick, accurate answers to everyday workplace questions about IT support, HR and benefits, and travel and expenses. Be concise and friendly, and point people to the right team when a request needs a human.
```

**Instructions to add (Lab 1):**
```
- Answer only from your configured knowledge. If you don't know, say so and suggest who to contact.
- Do not give legal, medical, or financial advice.
- Keep answers short. Use bullet points for lists.
```

**Build a topic with Copilot (Lab 2):**
```
Ask the employee for their name, a short description of their IT issue, and how urgent it is (High, Medium, or Low).
```
```
Add an adaptive card at the end that summarizes the name, issue, and urgency that were collected.
```

**Knowledge-source descriptions (knowledge demo/practice) — write one per source so the agent knows when to use it:**
```
IT Support FAQ — passwords, MFA, VPN, devices, software, and logging IT tickets.
HR & Benefits FAQ — PTO and leave, benefits enrollment, payroll, and remote work policy.
Travel & Expense Policy — what employees can claim, limits, per diem, and the expense workflow.
```

**Prompt tool instructions (Lab 3) — turns a rough problem into a clean ticket:**
```
Turn a user's rough description of an IT problem into a clean support ticket.
Output: Title (one line), Category (Access, Hardware, Software, Network, Other), Summary (two sentences).
```

**Trigger instruction (Lab 3) — what the agent does when an email arrives:**
```
When an email arrives about an IT problem, use the Draft IT Ticket tool on the email body and show the ticket. Ignore emails that aren't IT-related.
```

**Chain the flow after the ticket (Lab 4) — agent instruction:**
```
When a user reports an IT problem, use Draft IT Ticket to create the ticket 