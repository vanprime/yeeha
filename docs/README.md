# yeeha

A browser extension that enhances Jira without paid plugins or external services. All data stays in your browser - no third-party servers, no data exports.

# installation

## Chrome

1. Download the latest [release](https://github.com/vanprime/yeeha/releases)

2. Access the Extension Management Page: Type `chrome://extensions/` into your browser's address bar.
   <img src="./docs/images/chrome-extension-manager.png" width="300">

3. Enable Developer Mode: In the top right corner of the Extensions page, you'll see a toggle for `Developer mode`. Turn it on. This allows you to _load unpacked extensions_.<br>
   <img src="./docs/images/chrome-developer-mode.png" width="300">

3. Load the Unpacked Extension: Click on the load unpacked button which will appear in the corner after you enable Developer Mode. A file dialog will open. Navigate to your download of the [release](https://github.com/vanprime/yeehareleases).<br>
   <img src="./docs/images/chrome-load-unpacked.png" width="300">

4. pin the extension to the toolbar<br>
   <img src="./docs/images/chrome-pin-extension.png" width="300">

5. click the extension icon to toggle the side panel and start using the different features.

## Firefox

Mozilla requires all extensions to be signed for Release and Beta versions of Firefox. For development and testing, you have these options:

### Option A: Use Firefox Developer Edition or Nightly (Recommended)

1. Download [Firefox Developer Edition](https://www.mozilla.org/en-US/firefox/developer/) or [Firefox Nightly](https://www.mozilla.org/en-US/firefox/channel/desktop/)
2. Navigate to `about:config` and set `xpinstall.signatures.required` to `false`
3. Go to `about:debugging#/runtime/this-firefox`
4. Click "Load Temporary Add-on" and select the `manifest.json` from the `firefox-mv3` folder in the [release](https://github.com/vanprime/yeeha/releases)

### Option B: Install from ZIP (Temporary)

1. Download the Firefox ZIP from the [releases page](https://github.com/vanprime/yeeha/releases)
2. In Firefox, go to `about:debugging#/runtime/this-firefox`
3. Click "Load Temporary Add-on" and select any file from the extracted ZIP
4. Note: The extension will be removed when Firefox restarts

### Option C: Firefox ESR with Signature Check Disabled

1. Install [Firefox ESR](https://www.mozilla.org/en-US/firefox/enterprise/)
2. Navigate to `about:config`
3. Set `xpinstall.signatures.required` to `false`
4. Drag and drop the Firefox ZIP onto Firefox to install

# table of contents

- [Installation](#installation)
  - [Chrome](#chrome)
  - [Firefox](#firefox)
- [Motivation](#motivation)
- [Features](#features)
  - [Bulk Add Sub-tasks](#bulk-add-sub-tasks)
  - [Epic Audit](#epic-audit)
  - [Data Analysis](#data-analysis)
  - [Migrate Issues](#migrate-issues)
- [Authentication & Privacy](#authentication--privacy)

# motivation

Jira lacks built-in tools to inspect and analyze its own data. Most solutions require paid plugins or exporting to external services.

This extension runs entirely in your browser, communicates only with your Jira instance, and uses your existing session — no tokens, no external services, no data leaving your machine.


# features

## Bulk Add Sub-tasks

Create multiple sub-tasks for any Jira issue at once.

**Prerequisites:** Navigate to a Jira issue page whose type supports sub-tasks (Stories, Tasks — not Epics or sub-tasks) and whose project has sub-tasks enabled.

**Usage:**
1. Open the extension side panel → **Bulk Sub-tasks**
2. Enter sub-task titles separated by **semicolons** (`;`) or **newlines**
3. Click **Create Sub-tasks** — progress is shown in real time

**Common blockers:**
- "Subtasks cannot have their own subtasks" — you're on a sub-task issue
- "This issue type uses child issues instead of subtasks" — Epics don't use sub-tasks
- "Project doesn't have subtasks enabled" — ask your Jira admin

---

## Epic Audit

Audit epic health across a project or JQL query — child counts, open vs. done breakdown, and blocking linked issues.

**Prerequisites:** Navigate to a Jira **project page** (auto-detects project key) or an **advanced search** (`/issues`) with a JQL query.

**Usage:**
1. Open the extension side panel → **Epic Audit**
2. Choose **Audit project** or **Audit via JQL** depending on your context
3. Results show an overview grid (epics grouped by status category) and a detailed breakdown per epic with child counts and linked issues
4. Click **View open issues** or **View all issues** on any epic card to open a JQL search in Jira

---

## Data Analysis

Create datasets from JQL queries, organize them into projects, and generate visualizations and forecasts.

**Usage:**
1. Open the extension side panel → **Data Analysis**
2. Create a dataset by entering a JQL query
3. The extension fetches and processes matching issues
4. Visualize, analyze, and forecast from the resulting data

---

## Migrate Issues

Copy issues between Jira instances via a guided four-step flow.

**Prerequisites:**
- Access to both a **source** and **target** Jira instance (Cloud, Server, or Data Center)
- **Read** on source, **Create issue** on target
- Optional: Assign Issues, Create Attachments, and Link Issues permissions on target

### Step 1 — Setup

Configure scope and source issue selection in one place.

| Setting | What it does |
|---------|-------------|
| **Include all subtasks** | Fetches sub-tasks for matched parent issues, preserving hierarchy |
| **Include all Epic children** | Fetches child issues for matched Epics (**enabled by default**) |
| **Mentioned links as remote links** | Converts Jira/Confluence/Figma URLs in descriptions and comments to remote links |
| **Source comments in summary** | Includes up to 50 comments in the migration summary |
| **Migrate attachments** | Uploads attachments to target; falls back to remote links if upload fails |
| **SCM branches and pull requests** | Adds branch/PR data to the summary comment (GitHub or GitLab, **Server/DC only**) |

1. Choose one scope method:
   - **Drop Issues** (default): drag Jira issue links into the drop zone. Source instance is inferred from links, scope is generated as `key in (...)`, and each new drop appends to the existing dropped scope.
   - **Use JQL**: enter JQL manually or click **Refresh from tab** to capture JQL and infer source instance from the active Jira tab.
2. Fetch issues (drop mode auto-fetches; JQL mode uses **Fetch Issues**).
3. Validate the cached issue summary table before moving on.
4. The extension auto-detects estimation fields (e.g., Story Points) and epic link configuration.

### Step 2 — Target

1. Switch to the target Jira tab → click **Set as Target**
2. Select the target project key
3. Click **Load Configuration** — the extension checks permissions, issue types, fields, workflows, components, and estimation/epic link setup. Results are shown step-by-step with status icons

### Step 3 — Review & Fix

Map source data to target equivalents. Use **Auto-map all** to match by name, then review:

1. **Issue Type Mapping** — assign a target type for every source type (unmapped types block migration)
2. **Field Mapping** — map fields; required target fields are highlighted. Sub-mappings for priority, components, labels, and picklists appear inline
3. **Status Mapping** (optional) — map source statuses to target statuses per issue type. Uses workflow transition graphs to execute transitions after creation
4. **Assignee Mapping** (optional) — match source users to target users
5. **Preview & Validate** — filter by ready/errors/skipped, toggle individual issues

### Step 4 — Migrate

**Your active tab must be on the target Jira instance.**

Execution order: create components → epics → standard issues → sub-tasks → transition statuses → remote links → attachments → enrich (comments, assignees).

Progress is shown in real time. You can **Stop Migration** at any time; already-created issues remain.

**After completion:**
- Created/failed/skipped counts with detailed breakdowns
- **Copy CSV** to export results
- **Open in Jira Search** to view created issues

### First migration checklist

1. Setup: source scope defined (drop links or JQL), source context inferred, issues fetched
2. Target: target instance captured, project selected, configuration loaded
3. Review: issue types + required fields mapped, at least one issue ready
4. Migrate: target tab active before clicking **Start Migration**

For blocker-specific fixes, see `docs/migrate-issues-troubleshooting.md`.

# authentication & privacy

**No API tokens or credentials required.** The extension uses your existing Jira session cookies — no setup, no stored credentials, and access is limited to what your Jira account can already see.

**Data storage:** All data lives in your browser's local storage (IndexedDB). Nothing is sent to external servers. Clear extension storage anytime via the control panel.
