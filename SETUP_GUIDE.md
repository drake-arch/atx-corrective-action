# ATX Error → Corrective Action System — Setup Guide

## What Was Built

Two files live in this folder:

| File | Purpose |
|------|---------|
| `index.html` | Corrective action form (all 4 sections from ATX_Corrective Action doc) |
| `trend-analysis.html` | Weekly error rate dashboard — pulls live from ClickUp |

---

## Step 1 — Host the Form on GitHub Pages (5 minutes)

The form needs a public URL so the Slack link works from anywhere. GitHub Pages is free.

1. Go to github.com → New repository → Name it `atx-corrective-action` → Set to **Public**
2. Upload `index.html` to the repo (drag and drop, or use the web editor)
3. Go to **Settings → Pages → Source → Deploy from branch → main**
4. Your form will be live at:
   `https://[your-github-username].github.io/atx-corrective-action/`

---

## Step 2 — Add Your ClickUp API Token to the Form

1. In ClickUp go to: **Settings → Apps → API Token → Generate**
2. Copy the token (starts with `pk_`)
3. Open `index.html` in a text editor
4. Find line: `const CLICKUP_API_TOKEN = 'YOUR_CLICKUP_API_TOKEN_HERE';`
5. Replace with your actual token, e.g.: `const CLICKUP_API_TOKEN = 'pk_abc123...';`
6. Re-upload the updated file to GitHub

---

## Step 3 — Update Your ClickUp → Slack Automation

This is where the "Fill out corrective action" link gets added to your existing Slack notification.

1. In ClickUp, go to: **Operations Space → Quality Control → Error Tracking Form**
2. Click **Automations** (lightning bolt icon)
3. Find your existing "New task → Send Slack message" automation
4. Edit the message body — add this line at the bottom:

```
📋 Fill out corrective action → https://[your-github-username].github.io/atx-corrective-action/?task_id={{task.id}}&task_name={{task.name}}
```

Replace `[your-github-username]` with your actual GitHub username.

**How it works:** When someone clicks the link, the form opens pre-linked to that specific error task. When they submit, the corrective action is posted as a comment directly on the ClickUp task.

---

## Step 4 — Run the Weekly Trend Analysis

1. Open `trend-analysis.html` in any browser (or host it on GitHub Pages too)
2. Enter your ClickUp API token (same one from Step 2)
3. Choose how many weeks to display
4. Click **Run Analysis**

The dashboard will pull all errors from your Error Tracking Form list and show:
- Total errors, weekly average, 4-week trend, open CAs
- Stacked bar chart by status (new / CA in progress / resolved / complete)
- Error rate % once you enter weekly order counts manually
- Status breakdown donut chart
- Last 4 weeks vs prior 4 weeks comparison
- Recent errors table (last 30 days)

**To get error rate:** After the dashboard loads, scroll to "Enter Weekly Order Counts," type your total orders for each week, and click **Calculate Error Rates**. The rate chart and table will update instantly.

---

## How the Full Workflow Works (End to End)

```
Error submitted in ClickUp form
         ↓
ClickUp automation fires → Slack message sent to your channel
         ↓
Slack message now contains:
  "📋 Fill out corrective action → [link]"
         ↓
Manager clicks link → ATX Corrective Action form opens
  (pre-filled with task ID — form shows "Linked Error: [order #]")
         ↓
Manager fills out all 4 sections → clicks Submit
         ↓
Corrective action saved as formatted comment on the ClickUp error task
         ↓
Every week: open trend-analysis.html, run analysis,
  enter order counts → see error rate and trends
```

---

## ClickUp Task Statuses (for reference)

Your Error Tracking Form uses these statuses:
- **new error reported** — just submitted, needs review
- **data validated** — reviewed and confirmed
- **corrective action ip** — CA is in progress
- **resolved** — issue resolved
- **complete** — fully closed out

---

## Questions / Issues

Contact: drake@atomixlogistics.com
