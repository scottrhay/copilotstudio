# Create Agents in Microsoft Copilot Studio

Hands-on labs for the instructor-led course **Create Agents in Microsoft Copilot Studio** (based on Microsoft PL-7008), delivered by **Scott Hay, MCT**.

Across four labs you build one agent — the **HP Workplace Assistant** — an internal helper that answers employee questions across IT, HR, and travel/expense, runs a structured intake, and turns a meeting into action items posted to Teams. Each lab is self-contained: if you fall behind, start fresh at the next one.

## The labs

| Lab | Session | Time | You build |
|---|---|---|---|
| [Lab 1 — Create the agent](Instructions/Labs/01-create-agent.md) | 1 | ~30 min | Create the agent, add instructions and a knowledge source, publish to a demo website |
| [Lab 2 — Add a topic](Instructions/Labs/02-add-topic.md) | 1 | ~30 min | A structured "Log IT Request" intake with an Adaptive Card summary |
| [Lab 3 — Answer across sources](Instructions/Labs/03-add-knowledge.md) | 2 | ~30 min | Multi-source grounding: one cited answer synthesized from three documents |
| [Lab 4 — Turn a meeting into tasks](Instructions/Labs/04-meeting-tasks.md) | 2 | ~30 min | Generative extraction + a workflow tool that posts action items to Teams |

Every lab ends with optional **"Finished early?" stretch goals** — extra functionality, topic chaining, live website grounding, and agent evaluations.

## What you need

- A Microsoft 365 Copilot license and access to **Copilot Studio** at `https://copilotstudio.microsoft.com/` (your organization's default environment — no setup lab required)
- The sample data files in the [**Files**](Files/) folder (fictional sample data created for this class)
- For Lab 4: a Teams channel you can post to

## Folder guide

| Folder | Contents |
|---|---|
| [`Instructions/Labs/`](Instructions/Labs/) | The four lab guides |
| [`Files/`](Files/) | Sample documents you upload as agent knowledge during the labs |
| [`Handouts/`](Handouts/) | One-page quick reference: decision card, agent anatomy, copy-ready prompts |

## After class

The agent you build is a reusable pattern. Point it at your own team's documents, rebuild the intake topic for your own process, and use the evaluation stretch goals as your regression suite before every change.
