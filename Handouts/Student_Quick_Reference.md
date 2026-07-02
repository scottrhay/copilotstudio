# Copilot Studio — Student Quick Reference (HP)

**PL-7008 · Create agents in Microsoft Copilot Studio · Two-session HP delivery**

Today you build one agent — the **HP Workplace Assistant** — across four short labs. Keep this beside you.

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
**Scope note:** this course builds **agents**. Power Automate is *out of scope* — it shows up only as a ready-made **workflow your agent calls as a tool** (Lab 4).

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
| 2 | A **Log IT Request** topic (name, issue, urgency + summary card) | Structured conversations |
| 3 | **Three** knowledge sources (IT, HR, Travel) | Answer across sources, synthesize |
| 4 | **Meeting Tasks** — extract action items from a transcript, post to Teams | Make the agent take an action |

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

**Knowledge-source descriptions (Lab 3) — write one per source so the agent knows when to use it:**
```
IT Support FAQ — passwords, MFA, VPN, devices, software, and logging IT tickets.
HR & Benefits FAQ — PTO and leave, benefits enrollment, payroll, and remote work policy.
Travel & Expense Policy — what employees can claim, limits, per diem, and the expense workflow.
```

**Cross-source test (Lab 3):**
```
I'm a new hire this week. What do I need to sort out for IT and for HR?
```

**Meeting to tasks (Lab 4):**
```
Turn these meeting notes into action items
```
*(then paste your notes; review the owners and due dates, and tell it to post them to Teams.)*

---

## 6. Things that trip people up

- **Indexing takes time.** After you add a file/website, it can take 10+ minutes to be ready. Empty answers usually mean "still indexing," not "broken."
- **Right environment.** If your agent "disappears," check the environment selector (top-right) — you probably switched.
- **Name with your initials** if you share an environment.
- **Teams popup blocked? (Lab 4)** Click the blocked-popup icon in the address bar and allow pop-ups from `copilotstudio.microsoft.com`.
- **One sheet for Excel** when you upload a spreadsheet as knowledge.

---

## 7. Take it back to work

Start with one agent that answers from a document your team uses constantly. Add a topic for your most common intake. Add a workflow tool only once the conversation works. That's the HP Workplace Assistant — scaled to your team.
