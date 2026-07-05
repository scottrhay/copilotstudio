# Create Agents in Microsoft Copilot Studio

Hands-on labs for the instructor-led course **Create Agents in Microsoft Copilot Studio** (based on Microsoft PL-7008), delivered by **Scott Hay, MCT**.

You build **one agent** — the **HP Workplace Assistant** — and grow it across the labs: create and ground it, give it a structured intake topic, add tools and a trigger, run a multi-step flow, (optionally) connect a second agent, and measure it. Only the first lab overlaps what M365 Agent Builder can do — everything after is Copilot Studio only. Each lab is self-contained: if you fall behind, start fresh at the top of the next one.

These labs teach Copilot Studio as covered in the official Microsoft Learn path: [Create agents in Microsoft Copilot Studio](https://learn.microsoft.com/en-us/training/paths/create-extend-custom-copilots-microsoft-copilot-studio/) — use it for self-paced study before or after class.

## The labs

| Lab | Session | Time | You build |
|---|---|---|---|
| [Lab 1 — Create the agent](Instructions/Labs/01-create-agent.md) | 1 | ~30 min | Create the agent, add instructions and a knowledge source, publish to a demo website, and evaluate it |
| [Lab 2 — Add a topic](Instructions/Labs/02-add-topic.md) | 1 | ~35 min | A structured **Log IT Request** intake with **entities, variables, and a branch** |
| [Lab 3 — Tools + triggers](Instructions/Labs/03-tools-and-triggers.md) | 2 | ~30 min | A **Prompt tool** that drafts an IT ticket, plus an **event trigger** that fires on its own |
| [Lab 4 — Log and route a ticket](Instructions/Labs/04-log-and-route-ticket.md) | 2 | ~35 min | A **multi-step flow**: log the ticket to Excel, email the requester, and escalate to the IT lead if urgent |
| [Lab 5 — Connect a second agent](Instructions/Labs/05-connected-agents.md) *(optional)* | 2 | ~25 min | **Multi-agent routing**: build an HR specialist and route to it |
| [Lab 6 — Measure it](Instructions/Labs/06-analytics-and-feedback.md) *(optional)* | 2 | ~15 min | **Analytics & feedback**: seed the demo site, leave thumbs up/down, tour the Analytics tab |

**Knowledge** — answering across IT, HR, and Travel sources — is demonstrated in class with an optional short practice; it isn't a numbered lab.

> **Heads-up on Analytics:** the **Analytics** tab only counts real conversations on a **published channel** (the Demo website) — your **Test pane** chats won't appear there — and data aggregates about once a day. That's what Lab 6 walks through.

**Optional alternative:** [Lab 4B — Turn a meeting into tasks](Instructions/Labs/04b-meeting-tasks.md) teaches the same flow skills (multi-step + a condition) in a meeting-notes scenario. Run it instead of, or in addition to, Lab 4.

Every lab ends with optional **"Finished early?" stretch goals** — extra functionality, topic chaining, live website grounding, connectors, Power Fx, and agent evaluations.

## What you need

- A Microsoft 365 Copilot license and access to **Copilot Studio** at `https://copilotstudio.microsoft.com/` (your organization's default environment — no setup lab required).
- The sample data files in the [**Files**](Files/) folder (fictional sample data created for this class).
- **Lab 4:** upload **`HP_IT_Tickets.xlsx`** to your own **OneDrive** (the flow logs rows into it), and sign in to the **Excel Online (Business)** and **Office 365 Outlook** connectors when prompted.
- **Lab 4B / Lab 4 stretch:** a Teams channel you can post to.

## Folder guide

| Folder | Contents |
|---|---|
| [`Instructions/Labs/`](Instructions/Labs/) | The lab guides (Labs 1–6, plus the optional Lab 4B) |
| [`Files/`](Files/) | Sample documents you upload as agent knowledge, plus the Lab 4 ticket tracker |
| [`Handouts/`](Handouts/) | One-page quick reference and a copy-paste **prompt packet** of every prompt used in the labs |

## After class

The agent you build is a reusable pattern. Point it at your own team's documents, rebuild the intake topic for your own process, swap the ticket tracker and approver for your real ones, and use the evaluation stretch goals as your regression suite before every change.
