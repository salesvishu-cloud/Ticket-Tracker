# Ticket Control Center

Ticket management and delay-analysis dashboard for e-commerce operations teams handling **Amazon**, **Flipkart** and **Direct Fulfillment (DF)** tickets.

One file, no build step, no server — `index.html` runs in any modern browser.

---

## What it does

| Section | Answers |
|---|---|
| **Dashboard** | Total / Open / Work in Progress / Waiting for Our Action / Waiting for Platform / Follow-up Due / Overdue / Resolved — every card opens the matching filtered list |
| **Management View** | The whole position in ~30 seconds: open, overdue, internal vs platform backlog, critical and oldest tickets, average resolution time |
| **All Tickets** | Ticket ID, Platform, Issue, Status, Waiting With, Assigned To, Created On, Days Open, Last Update, Next Follow-up, Delay Reason, Priority — with 14 filters and global search |
| **Action Today** | Urgency-sorted working queue with an *Action Required* column, plus 3 / 5 / 7-day no-update alerts |
| **Delays & SLA** | Delay reasons by count and total delay days, delay owner, "Waiting With" split, SLA health by priority |
| **Ticket Ageing** | 0–2 / 3–5 / 6–10 / 11–15 / 16–30 / 30+ day buckets and the oldest open tickets |
| **Activity Log** | Every reply, follow-up, escalation and resolution across all tickets |
| **Reports** | Daily, Weekly and Monthly Management reports — Excel, CSV and PDF (print) |
| **Import** | Reads the existing Excel sheet (S.No, Ticket ID, Creation Date, End Date, Platform, Issue, What Updates, Remarks) |
| **Settings** | SLA targets, alert thresholds, team list, notifications, shared-data connection, backup |

Clicking a Ticket ID opens its full profile with a chronological timeline.

---

## Where the data is stored

GitHub Pages only serves the page — it cannot store data. **Tickets are shared through a Google Sheet.** Until the sheet is connected, each person's tickets stay inside their own browser and nobody else can see them. The top bar always shows which mode you are in:

| Top-bar chip | Meaning |
|---|---|
| 🟢 **Shared sheet · 12:57** | Connected to the team's Google Sheet; last synced at that time |
| 🟡 **This browser only** | Not connected — colleagues cannot see these tickets |
| 🔴 **Sync failed** | Sheet unreachable; changes are queued and upload automatically when it works again |

### Connect the shared Google Sheet (once, ~5 minutes)

The Apps Script file (`Code.gs`) is **not** part of this repository — it holds the team key, so it is kept out of public GitHub on purpose.

1. Create a new Google Sheet (use a company Google account).
2. **Extensions → Apps Script**. Replace everything with the contents of `Code.gs`.
3. Change `TEAM_KEY` at the top to a password your team will share. Save.
4. **Deploy → New deployment → Web app** — *Execute as*: **Me**, *Who has access*: **Anyone**. Deploy and allow the permissions.
5. Copy the Web App URL (ends in `/exec`).
6. Open the dashboard → **Settings → Shared data** → paste the URL and team key → **Connect & sync**.
7. Give each colleague the URL and key once; they do step 6 in their own browser.

Tickets raised in a browser before it was connected show in a blue banner with **Upload to shared sheet**, so nothing is lost.

### How syncing works
- Every save goes to the sheet immediately; the page pulls teammates' changes every 60 seconds and whenever you return to the tab.
- If the sheet can't be reached, changes wait in the browser and upload on the next successful sync.
- The team key is kept only in each person's browser — never in this repository.

---

## Calculated automatically

- **Ticket Age** — open: today − creation date · closed: end date − creation date
- **Target Resolution Date** — creation date + SLA days for the priority (unless set on the ticket)
- **Delay Days** — days past target, otherwise 0
- **SLA Status** — On Track / Due Soon / Due Today / Overdue / Closed
- **Waiting With** — Our Side / Platform Side / Other
- **No-update alerts** — 3 / 5 / 7+ days (configurable)
- **Smart status suggestion** from the latest activity, with manual override

---

## Roles

**Admin** (everything) · **Manager** (all except clearing data) · **Team Member** (raise tickets, add activity) · **Viewer** (read-only). Roles shape the interface; they are not a security boundary on a public URL.

## Notifications

The in-app alert bell works out of the box. The Email and WhatsApp toggles store a preference only — a static page cannot send messages by itself.

## Tech

Plain HTML, CSS and JavaScript in one file. External resources: Google Fonts and [SheetJS](https://sheetjs.com/) (Excel import/export) from a CDN. Light and dark themes.
