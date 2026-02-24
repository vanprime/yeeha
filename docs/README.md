# yeeha

A browser extension that enhances Jira without paid plugins or external services.

## Data privacy

**No API tokens or credentials required.** The extension uses your existing Jira session cookies (`credentials: "include"`) —  so there is no setup and no stored credentials.

### Network calls

The extension only communicates with:

| Destination | Methods | What for |
|---|---|---|
| Your **source Jira instance** | `GET` against `/rest/api/2–3/search`, `/issue/{key}`, `/issue/{key}/comment`, `/issue/{key}/remotelink`, `/project`, `/issuetype`, `/serverInfo`, `/rest/agile/1.0/board`, `/rest/dev-status/latest/issue/*` | Read issues, comments, attachments, workflows, projects, boards, SCM data |
| Your **target Jira instance** | `GET` + `POST` + `PUT` against `/rest/api/2–3/issue`, `/issue/bulk`, `/issue/{key}/transitions`, `/issue/{key}/remotelink`, `/issue/{key}/attachments`, `/component`, `/mypermissions`, `/workflow/search`, `/user/assignable/search` | Create/update issues, upload attachments, execute transitions, check permissions |
| `api.github.com` | `GET /repos/vanprime/yeeha/releases` | Extension update check (read-only, no user data sent) |

There are no analytics, telemetry, crash reporting, or any other external calls.

### Data storage

All data stays in your browser:

| Mechanism | What it stores |
|---|---|
| **IndexedDB** `yeeha-migrate-issues` | Migration run history, per-issue results, resume checkpoints, lineage records (source-to-target key mappings), status-sync action results |
| **IndexedDB** `yeeha-data-analysis` | Fetched issue data for analysis datasets |
| **chrome.storage.local** | Migration wizard state, data-analysis project/dataset metadata |
| **localStorage** | UI preferences only (theme, font scale, panel visibility) |

Clear extension storage anytime via the control panel at the top.

---

# Installation

## Chrome

1. Download the latest [release](https://github.com/vanprime/yeeha/releases)
2. Go to `chrome://extensions/` and enable **Developer mode**
   <img src="./docs/images/chrome-developer-mode.png" width="300">
3. Click **Load unpacked** and select the downloaded release folder
   <img src="./docs/images/chrome-load-unpacked.png" width="300">
4. Pin the extension to the toolbar
   <img src="./docs/images/chrome-pin-extension.png" width="300">
5. Click the extension icon to open the side panel

## Firefox (untested)

Mozilla requires extensions to be signed. Options for development/testing:

### Option A: Firefox Developer Edition or Nightly (Recommended)

1. Download [Firefox Developer Edition](https://www.mozilla.org/en-US/firefox/developer/) or [Firefox Nightly](https://www.mozilla.org/en-US/firefox/channel/desktop/)
2. Set `xpinstall.signatures.required` to `false` in `about:config`
3. Go to `about:debugging#/runtime/this-firefox` → **Load Temporary Add-on** → select `manifest.json` from the `firefox-mv3` folder

### Option B: Temporary install from ZIP

1. Download the Firefox ZIP from the [releases page](https://github.com/vanprime/yeeha/releases)
2. Go to `about:debugging#/runtime/this-firefox` → **Load Temporary Add-on** → select any file from the ZIP
3. Note: removed on Firefox restart

### Option C: Firefox ESR

1. Install [Firefox ESR](https://www.mozilla.org/en-US/firefox/enterprise/)
2. Set `xpinstall.signatures.required` to `false` in `about:config`
3. Drag and drop the Firefox ZIP onto Firefox

---

# Features

## Bulk Add Sub-tasks

Create multiple sub-tasks for any Jira issue at once. Navigate to an issue that supports sub-tasks (Stories, Tasks), enter titles separated by semicolons or newlines, and click **Create Sub-tasks**.

## Epic Audit

Audit a project/space and analyse its epics. Child counts, open vs. done breakdown, and blocking linked issues. To use it, just navigate to any page of your Jira instance.

## Data Analysis

Create datasets from JQL queries, organize them into projects, and generate visualizations and forecasts.

## Migrate Issues

Copy issues between Jira instances (Cloud, Server, or Data Center) via a guided four-step wizard: **Setup** (define source scope via drag-and-drop or JQL) → **Target** (select target project and load its configuration) → **Review** (map issue types, fields, statuses, and assignees) → **Migrate** (execute with real-time progress). The extension preserves hierarchy (epics → stories → sub-tasks), migrates attachments, comments, and remote links, and can transition issues to mapped target statuses using workflow graphs. Migrations are resumable — if interrupted, pick up where you left off from the welcome panel or the **History & Sync** page.

### History & Sync

The **History & Sync** page shows completed and interrupted migration runs. For completed runs, you can verify target issue statuses against your mappings and apply a **Sync statuses** action to re-transition issues that have drifted. Interrupted runs appear in a dedicated section with per-issue stage details and a link to resume.
