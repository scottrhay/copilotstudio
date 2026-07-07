# Lab 4 — Log and route an IT ticket (a multi-step flow)

**Course:** PL-7008 — Create agents in Microsoft Copilot Studio
**Session:** 2
**Time:** ~35 minutes

> **Lab Scenario.** In Lab 3 your agent *drafts* a clean IT ticket. Drafting isn't doing. A **flow** is how an agent does **several things in one run** — that's the whole point of a flow, not a single button. You'll build a **Log and Route Ticket** flow that (1) **logs the ticket to an Excel tracker**, (2) **emails the requester a confirmation**, and (3) **branches** — if the ticket is High urgency, it *also* alerts the IT lead. One call from the agent, three actions, real logic. This is the capstone of the IT-support assistant you've built since Lab 1.

## Quick start
- Open your **HP Workplace Assistant** from Labs 1–3 — it should already **draft clean tickets from its instructions** (Lab 3, Part A). *If you're starting fresh:* create the agent, then add the drafting instruction first (it's in Lab 3's Part A).
- Download **`HP_IT_Tickets.xlsx`** from the class files and **upload it to your OneDrive** — the flow logs rows into it. It already contains a table named **`Tickets`**.
- You'll use the **Excel Online (Business)** and **Office 365 Outlook** connectors — sign in when prompted.

> **Terminology:** Microsoft is renaming *agent flows* to *Copilot Studio workflows*. You'll see both terms — they're the same thing.

---

## Your turn

### Step 1 — Prep the ticket tracker
1. Download **`HP_IT_Tickets.xlsx`** and **upload it to your OneDrive** (app launcher → **OneDrive** → **Upload**).
2. Open it and confirm the **`Tickets`** table with columns **Date, Title, Category, Urgency, Summary, Requester, Status** (one sample row is already there). Close the tab.

### Step 2 — Create the flow and its inputs
1. In your agent: left navigation → **Tools** → **+ New tool** → **Agent flow** tile.
2. Confirm the **When an agent calls the flow** trigger and the **Respond to the agent** action are present.
3. Select the **trigger** → **+ Add an input** → **Text** — add **five**: `Title`, `Category`, `Urgency`, `Summary`, `Requester`. **Save draft**.
4. **Overview** tab → **Details** → **Edit**: **Flow name** `Log and Route Ticket`; **Description** `Log an IT ticket to Excel, email the requester, and alert the IT lead when the ticket is urgent.` → **Save**.

### Step 3 — Action 1: log the ticket to Excel
1. **Designer** tab → **+** after the trigger → search `Excel` → **Excel Online (Business)** → **Add a row into a table** → **Sign in** and approve.
2. **Location:** `OneDrive for Business` · **Document Library:** `OneDrive` · **File:** browse to `HP_IT_Tickets.xlsx` · **Table:** `Tickets`.
3. Map the columns to **Dynamic content** from the trigger:
   - **Title** → `Title` · **Category** → `Category` · **Urgency** → `Urgency` · **Summary** → `Summary` · **Requester** → `Requester`
   - **Status** → type `Open`
   - **Date** → in the field, click **fx** (expression) and enter `utcNow()` → **OK**. *(Optional — leave blank if you'd rather skip it.)*

### Step 4 — Action 2: email the requester a confirmation
1. **+** below the Excel step → search `Outlook` → **Office 365 Outlook** → **Send an email (V2)** → **Sign in** and approve.
2. **To:** click **fx** (expression) and wrap the requester in `trim(...)` — e.g. `trim(triggerBody()?['Requester'])`. *(The AI sometimes appends a newline to the email address; `trim` strips it. Without it the send fails with a generic **SystemError**. No requester handy in class? Use your own email.)*
3. **Subject:** `Your IT ticket has been logged: ` then add the **Title** dynamic content.
4. **Body:** a short confirmation — e.g. `We've logged your ticket.` then add **Category**, **Urgency**, and **Summary** dynamic content on their own lines.

### Step 5 — Action 3: branch — alert the IT lead if urgent
1. **+** below the email step → **Control** → **Condition**.
2. Set it: **Urgency** *(dynamic content)* **is equal to** `High`.
3. On the **If yes** branch → **+** → **Office 365 Outlook** → **Send an email (V2)**:
   - **To:** the IT lead's address *(use your own or a class address the instructor gives you)*
   - **Subject:** `⚠️ High-priority IT ticket: ` + **Title**
   - **Body:** add **Summary** and **Requester**.
4. Leave the **If no** branch empty. **This is the flow's logic** — routine tickets are logged and confirmed; only urgent ones escalate.

### Step 6 — Respond and publish
1. Select **Respond to the agent** → **+ Add an output** → **Text**, name `Result`, value `Ticket logged and routed.`
2. **Save draft** → **Publish**. Confirm the tool shows **Ready**.

### Step 7 — Give the flow to the agent, chained after the ticket
1. **Agents** → **HP Workplace Assistant** → **Tools** tab → **+ Add a tool** → **Workflows** filter → **Log and Route Ticket** → **Add and configure**.
2. **Inputs → Fill using: Dynamically fill with AI** for all five. Expand **Additional details** to find **Ask the end user before running** → set it to **Yes** *(a safe default for anything that writes data and sends mail)*. Under **Completion → After running**, set **Write the response with generative AI**. **Save**.
3. **Overview → Instructions → Edit**, add and **Save**:
   ```
   When a user reports an IT problem, draft a clean ticket (Title, Category, Summary) and show it. After the user confirms, use Log and Route Ticket, passing the ticket's Title, Category, Urgency, and Summary (and the user's email as Requester if you have it).
   ```

### Step 8 — Test the whole chain
1. **Test** pane → **Show activity map** On → **Start new test session** (**+**).
2. Enter a messy, clearly urgent problem — **include your email so the flow has a requester to confirm to**:
   ```
   my laptop won't join the wifi and I have a client demo in an hour. My email is your.name@hp.com
   ```
3. The agent **drafts the ticket** (Draft IT Ticket), shows it, and asks you to confirm. Reply `Yes, log it.`
4. Confirm all three actions happened: a **new row** in `HP_IT_Tickets.xlsx`, a **confirmation email**, and — because this one is urgent — the **IT-lead alert email**.
5. Run it again with a low-urgency issue (`my mouse is running low on battery, no rush. My email is your.name@hp.com`). It logs and confirms, but **no alert fires** — the *If no* path. In the **activity map**, watch it happen in sequence: the agent **drafts** the ticket from its instructions, then calls the **flow** to log and route it. That chain — reason, then act in multiple steps — is what an agent with a flow can do.

---

## ⭐ Finished early? Optional stretch goals

*Pick one — all are self-contained and safe to skip.*

### Stretch A — Alert to Teams instead of email
On the **If yes** branch, replace (or add alongside) the alert email with **Microsoft Teams → Post message in a chat or channel** → post `⚠️ High-priority ticket: {Title}` to an IT channel. Same branch, a different destination — pick the channel your team actually watches.
> Teams popup blocked? Click the blocked-popup icon in the address bar and allow pop-ups from `copilotstudio.microsoft.com`.

### Stretch B — Read the tickets back
A flow can *fetch*, not just write. Build a second flow **`Get Open Tickets`**: trigger input `Category` (Text) → **Excel Online (Business) → List rows present in a table** on `HP_IT_Tickets.xlsx` / `Tickets` → **Filter query** `Status eq 'Open'` → **Respond to the agent** with the **body/value**. Add it to the agent and test: `Show me the open network tickets`. Now your assistant both logs and reports — the two halves of a real help desk.

### Stretch C — Try the alternative flow lab
Prefer a different scenario? **Lab 4B — Turn a meeting into tasks** teaches the same flow skills (multi-step + a condition) by extracting action items from a meeting transcript and posting them to Teams. Same muscle, different domain.

---

## ✅ Done — and how to reuse it
Your assistant now **drafts a ticket and acts on it** — logging to a system of record, confirming to the user, and escalating only what's urgent. **Reuse idea:** the log-and-route pattern fits any intake — facilities requests, access requests, purchase approvals. Swap the Excel tracker for your real list and the email for your real approver.

| Check | |
|---|---|
| `HP_IT_Tickets.xlsx` in **your OneDrive** with a **Tickets** table | ☐ |
| **Log and Route Ticket** flow built: **Excel add-row + Outlook email + a Condition branch**, and **Ready** | ☐ |
| Flow added to the agent; the agent chains **Draft IT Ticket → Log and Route Ticket** | ☐ |
| A test ticket produced a **new Excel row** and a **confirmation email** | ☐ |
| A **High-urgency** ticket also fired the **IT-lead alert**; a low-urgency one did not | ☐ |

*Look how far the assistant has come: it grounds on your documents (Lab 1), runs a structured intake (Lab 2), uses a tool and a trigger (Lab 3), and now runs a multi-step flow with logic (Lab 4). **Optional Lab 5** goes one step further — build a second, specialist agent and let this one route to it.*
