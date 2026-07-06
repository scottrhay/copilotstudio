# Lab 4B (optional) — Turn a meeting into tasks

**Course:** PL-7008 — Create agents in Microsoft Copilot Studio
**Session:** 2
**Time:** ~30 minutes — **optional alternative to Lab 4**

> **This is an alternative to Lab 4.** It teaches the **same flow skills** — a multi-step flow with a **condition** — in a different scenario. Run it *instead of* Lab 4 if a meeting-notes example lands better with your group, or *in addition* if you're ahead. Lab 4 (Log and Route an IT ticket) stays on the IT-support through-line; this one branches into meetings.

> **Lab Scenario.** A **flow** is the richest kind of tool — real logic and multiple actions, run by the agent. Here your HP Workplace Assistant **reads a meeting transcript, pulls out the action items, and calls a flow** that posts them to Teams **and branches:** if the meeting has an urgent item, it *also* flags leadership. Two skills: generative extraction from messy text, and a multi-step flow with a **condition** — pure Copilot Studio.

## Quick start (so everyone can begin)
- **If you have your agent:** open **HP Workplace Assistant**.
- **If you're starting fresh:** create an agent named `HP Workplace Assistant` with the prompt below, then continue:
  ```
  You are the HP Workplace Assistant. You help employees turn meeting notes into clear action items and post them to a Microsoft Teams channel.
  ```
- You need a **Teams channel you can post to** (your instructor will name a class team if needed).

> **Terminology:** Microsoft is renaming *agent flows* to *Copilot Studio workflows*. You'll see both terms.

---

## Your turn

### Step 1 — Teach it to extract action items
1. Open your agent → **Overview** tab → **Instructions** → **Edit**.
2. Add these lines, then **Save**:
   ```
   - When the user gives you meeting notes or a transcript, extract the action items.
   - For each action item, list the Owner, the Task, and the Due date. Note any dependency between tasks.
   - Show the action items as a numbered list and ask the user to confirm before doing anything else.
   ```
3. In **Suggested prompts**, add one — **Title:** `Meeting to tasks` · **Prompt:** `Turn these meeting notes into action items` · **Save**.

### Step 2 — Test the extraction
1. Open the **Test** pane → **Start new test session** (**+**).
2. Enter `Turn these meeting notes into action items`, then paste the sample transcript below (or open **`HP_Sample_Meeting_Transcript.docx`** and copy it):

   ```
   HP — Q3 New-Hire Onboarding Sync (June 26, 2026)
   Attendees: Priya (IT), Marcus (HR), Dana (Facilities), Sam (Procurement)

   Marcus (HR): The July cohort starts the 14th and last quarter felt rushed. Let's lock owners and dates.
   Priya (IT): Laptops arrived late last time. I'll pre-stage 20 laptops, but I need the order confirmed first. I can have them imaged by July 10.
   Sam (Procurement): I'll confirm the laptop order and send Priya the tracking numbers by July 3.
   Marcus (HR): Our onboarding checklist still references the old VPN process. Priya, send me the updated VPN steps and I'll refresh it.
   Priya (IT): I'll send the updated VPN setup steps by July 5.
   Marcus (HR): I'll update the onboarding portal checklist by July 8, after I get the VPN steps.
   Dana (Facilities): I'll assign desks and set up badge access for the cohort by July 9.
   Marcus (HR): I'll schedule the new-hire welcome session for July 14 and send invites by July 7.
   ```

3. Review what it extracted. You should see ~6 action items with an owner and a due date each, and it should note that the checklist update **depends on** the VPN steps. **This is the human-in-the-loop step** — you check the list before anything is posted.

> Model output varies. If owners or dates are missed, say `Include the owner and due date for each item` and it will refine.

### Step 3 — Build the "Post Tasks to Teams" flow — with logic
A good flow does more than one thing. Yours **always posts the tasks**, and **also flags leadership when a meeting has an urgent item** — a **condition**, which is what makes flows powerful.
1. Left navigation → **Tools** → **+ New tool** → **Agent flow** tile.
2. Confirm the **When an agent calls the flow** trigger and **Respond to the agent** action are present.
3. Select the **trigger** → **+ Add an input** → **Text** named `Task List`. Add a **second** input → **Text** named `Urgent` (the agent sets this to `Yes` or `No`). **Save draft**.
4. **Overview** tab → **Details** → **Edit**: **Flow name** `Post Tasks to Teams`; **Description** `Post meeting action items to Teams, and flag leadership when a meeting has an urgent item.` → **Save**.
5. **Designer** tab → **+** after the trigger → search `Teams` → **Microsoft Teams → Post message in a chat or channel** → **Sign in** and approve.
   > Popup blocked? Click the blocked-popup icon in the address bar and allow pop-ups from `copilotstudio.microsoft.com`.
6. **Post as** `Flow bot` · **Post in** `Channel` · pick a **Team** and **Channel** you belong to · **Message:** **Dynamic content** → **Task List**.
7. **Add the logic:** **+** below the Teams post → **Control → Condition**. Set it: **Urgent** *(dynamic content)* **is equal to** `Yes`.
8. On the **If yes** branch → **+** → **Microsoft Teams → Post message in a chat or channel** → same team/channel → **Message:** `⚠️ This meeting has an urgent action item — please review.` Leave **If no** empty.
9. Select **Respond to the agent** → **+ Add an output** → **Text** named `Result` = **Dynamic content** → the first post's **Message link**.
10. **Save draft** → **Publish**. Confirm the tool shows **Ready**.

### Step 4 — Give the tool to the assistant
1. **Agents** → open **HP Workplace Assistant** → **Tools** tab → **+ Add a tool** → **Workflows** filter → **Post Tasks to Teams** → **Add and configure**.
2. **Inputs → Fill using: Dynamically fill with AI** for **both** `Task List` and `Urgent`. **Ask the end user before running: No**. Under **Completion → After running**, set **Write the response with generative AI**. **Save**.
3. **Overview** → **Instructions** → **Edit**, add and **Save**:
   ```
   After the user confirms the action items, use the Post Tasks to Teams tool. Pass the confirmed list as Task List, and set Urgent to Yes if any item is due within two business days or marked high priority, otherwise No.
   ```

### Step 5 — Test both paths
1. **Test** pane → **Start new test session** (**+**). Paste the transcript, let it extract, then reply `Yes, post these to Teams.` (Select **Allow** if prompted to connect.)
2. Check Teams: the task list posts. Because the sample has near-term dates, the agent sets **Urgent = Yes**, so the **flag message also posts.** Open the flow's **run history** (or the **Activity** page) and see the **Condition** took the *If yes* branch.
3. Run it again, but tell the agent the items are all low priority (or paste a transcript with far-off dates). The flag should **not** post — the *If no* branch. One flow, two behaviors: that's flow logic.

---

## ⭐ Finished early? Optional stretch goals — if time permits

*Pick one — all are self-contained, optional, and safe to skip.*

### Stretch A — Harden the extraction
Real transcripts are messier than the sample. Make the agent defensive.
1. **Overview** → **Instructions** → **Edit**. Add, then **Save**:
   ```
   - Format every due date as YYYY-MM-DD.
   - If a task has no clear owner, mark the owner as UNASSIGNED rather than guessing.
   - Ignore ideas and suggestions ("we should probably...") unless someone explicitly commits to doing them.
   ```
2. Re-run the extraction, but first add these two lines to the end of the transcript:
   ```
   Dana (Facilities): We should probably repaint the training room at some point.
   Marcus (HR): Someone needs to order the welcome-kit swag before the 14th.
   ```
3. Check the result: the repaint *idea* should be excluded (or flagged as not committed), and the swag task should appear with owner **UNASSIGNED** and due **2026-07-14**.

### Stretch B — Retrieve data with a tool, not just post
So far your tool *sends*. Tools also *fetch* — the pattern behind every "look it up for me" agent.
1. Open OneDrive (app launcher, top-left → **OneDrive**) → create an **Excel workbook** named `Operations tasks` with columns `Title`, `Priority`, `Status` and 3–4 rows (mix of High and Medium). Select the data range → **Insert → Table** → name the table `Tasks`. Close the tab.
2. **Tools** → **+ New tool** → **Agent flow**. On the trigger, **+ Add an input** → **Text** named `Priority`.
3. Insert the **Excel Online (Business) → List rows present in a table** action → **Location:** OneDrive for Business → browse to your workbook → **Table:** `Tasks` → **Show all** → **Filter query:** `Priority eq ''` and place the **Priority** dynamic content between the quotes.
4. **Respond to the agent** → **+ Add an output** → **Text** named `Task list` = the **body/value** dynamic content. Name the flow `Get Task List`, description `Retrieve tasks matching a priority`. **Save draft** → **Publish**.
5. Add it to your agent as a tool (**Dynamically fill with AI**), then test: `Show me the high priority operations tasks`.

*Prefer Planner? The same pattern works with **Planner → Create a task** to turn each action item into a real Planner task.*

### Stretch C — Regression-test the whole assistant
Your agent now has knowledge, a topic, and a tool. Build one evaluation that exercises all of it.
1. **Evaluate** tab → new test set → add **4 manual test cases**: an IT question, an HR question, `I need to log an IT issue`, and `Turn these meeting notes into action items:` followed by two transcript lines.
2. **Run** it, then open each case and confirm the right capability fired — knowledge for the first two, the **Log IT Request** topic for the third, extraction for the fourth.
3. Export or keep the set: this is the regression suite you'd re-run before every future change to this agent.

> **Want to ship it for real?** If HP's tenant allows custom Teams apps, **Channels → Microsoft 365 and Microsoft Teams → Add channel** puts the assistant inside Teams, where employees actually live. Ask the instructor / check with IT before trying in class.

---

## ✅ Done — and how to reuse it
Your assistant now turns a meeting into posted tasks. **Reuse idea:** paste your own real meeting notes after any sync and let it draft the follow-ups — then send them to Teams, or extend it to Planner. Start with posting (low risk), then graduate to creating tasks.

| Check | |
|---|---|
| Instructions tell the agent to extract owner / task / due date and confirm first | ☐ |
| It extracted the action items from the sample transcript | ☐ |
| **Post Tasks to Teams** flow built with a **Condition** branch, and **Ready** | ☐ |
| Added to the assistant as a tool (both inputs filled by AI) | ☐ |
| The task list **posted to Teams**, and the **urgent flag** fired on the *If yes* path | ☐ |

*Same flow muscle as Lab 4 — a multi-step tool with a **condition** — in a different scenario. Whichever you ran, the assistant now takes real, multi-step action. **Optional Lab 5** goes further: connect a second, specialist agent and let this one route to it.*
