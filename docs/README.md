# jira-extension

A browser extension that enhances Jira without paid plugins or external services. All data stays in your browser - no third-party servers, no data exports.

# motivation

Jira (both on-premise and cloud) lacks built-in tools to properly inspect and analyze the data we generate while using the platform. Most solutions require either expensive plugins like ScriptRunner or exporting data to external services like spreadsheets, BI tools, or third-party analytics platforms.

We built this browser extension to solve that problem differently:

- **100% local**: All processing happens in your browser. Your data never leaves your machine.
- **No external services**: The extension only communicates with your Jira instance - nothing else.
- **No additional authentication**: Uses your existing Jira session, so there's nothing to configure.
- **Privacy by design**: Since everything runs locally, there's no risk of sensitive project data being transmitted to third parties.

This approach lets you unlock powerful features (bulk operations, epic auditing, data analysis, issue migration) while keeping full control over your data.

# usage

## Chrome

1. Access Extension Management Page: Type `chrome://extensions/` in the address bar and press Enter.<br>
   <img src="./docs/images/chrome-extension-manager.png" width="300">

2. Enable Developer Mode: In the top right corner of the Extensions page, you'll see a switch for Developer mode. Turn it on. This allows you to load unpacked extensions.<br>
   <img src="./docs/images/chrome-developer-mode.png" width="300">

3. Load the Unpacked Extension: Click on the load unpacked button which will appear in the corner after you enable Developer Mode. A file dialog will open. Navigate to your download of the [release](https://github.com/vanprime/jira-extension/releases).<br>
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
4. Click "Load Temporary Add-on" and select the `manifest.json` from the `firefox-mv3` folder in the [release](https://github.com/vanprime/jira-extension/releases)

### Option B: Install from ZIP (Temporary)

1. Download the Firefox ZIP from the [releases page](https://github.com/vanprime/jira-extension/releases)
2. In Firefox, go to `about:debugging#/runtime/this-firefox`
3. Click "Load Temporary Add-on" and select any file from the extracted ZIP
4. Note: The extension will be removed when Firefox restarts

### Option C: Firefox ESR with Signature Check Disabled

1. Install [Firefox ESR](https://www.mozilla.org/en-US/firefox/enterprise/)
2. Navigate to `about:config`
3. Set `xpinstall.signatures.required` to `false`
4. Drag and drop the Firefox ZIP onto Firefox to install


# features

## Bulk Add Sub-tasks

Create multiple sub-tasks for any Jira issue in one go.

**Prerequisites:**
- You must be on a Jira issue page (the extension detects the issue key from the URL)
- The issue type must support sub-tasks (e.g., Stories, Tasks — not Epics or other sub-tasks)
- The project must have sub-tasks enabled

**Step by step:**
1. Open any Jira issue in your browser
2. Open the extension side panel and navigate to **Bulk Sub-tasks**
3. The extension shows the detected issue key, type, and summary. If something is wrong (e.g., you're on a sub-task, or the project has sub-tasks disabled), a warning explains why
4. Enter sub-task titles separated by **semicolons** (`;`) or **newlines** — the count updates as you type
5. Click **Create Sub-tasks**. A progress bar tracks the four internal steps (metadata fetch → user info → issue construction → posting)
6. On success you see a confirmation with the number of created sub-tasks. On failure a specific error message is shown

**Common blockers:**
- "Subtasks cannot have their own subtasks" — you are on a sub-task issue
- "This issue type uses child issues instead of subtasks" — Epics and other parent-level types don't use sub-tasks
- "Project doesn't have subtasks enabled" — ask your Jira admin to enable sub-tasks for the project

---

## Epic Audit

Audit the health of epics across a project or a custom JQL query. See how many children each epic has, how many are open vs. done, and which linked issues might be blocking progress.

**Prerequisites:**
- Either navigate to a **Jira project** page (the extension picks up the project key), or
- Run an **advanced search** (`/issues`) with a JQL query — the extension reads the query from the URL

**Step by step:**
1. Open the extension side panel and navigate to **Epic Audit**
2. Depending on your context you see one or both options:
   - **Audit project `{KEY}`** — scans all epics in the detected project
   - **Audit via JQL** — uses the JQL from your active advanced search
3. The audit runs in multiple stages: searching for epics → analyzing results → fetching children → fetching linked issues. Progress is shown in real time
4. Results appear in two sections:
   - **Overview grid** — epics grouped by status category (e.g., Open, In Progress, Done) with counts
   - **Detailed breakdown** — each epic shows its key, summary, status, open/done child counts, and linked issues. Click an epic key to open it in Jira
5. At the bottom of each epic card you can **View open issues** or **View all issues**, which opens a pre-built JQL search in Jira

**Tip:** If you don't see any epics, double-check that the project actually contains epic-type issues and that your account has permission to view them.

---

## Data Analysis

Inspect and visualize the data hidden inside your Jira issues. Create datasets from JQL queries, organize them into projects, and generate visualizations and forecasts.

**Capabilities:**
- **Dataset Creation**: Import Jira issues via JQL into analyzable datasets
- **Project Library**: Organize multiple datasets into projects for better management
- **Visualization**: Generate charts and graphs from your Jira data
- **Forecasting**: Project future trends based on historical data

**How it works:**
1. Navigate to "Data Analysis" in the extension
2. Create a new dataset by entering a JQL query
3. The extension fetches and processes the matching issues
4. View, analyze, and visualize the resulting data

---

## Migrate Issues

Copy issues between Jira instances. A five-step wizard walks you through scoping, fetching source issues, configuring the target project, validating field mappings, and executing the migration.

**Prerequisites:**
- Access to both a **source** and a **target** Jira instance (can be Cloud, Server, or Data Center)
- **Read** permissions on the source project
- **Create issue** permissions on the target project
- For assignee migration: the target project must have assignable users and the "Assign Issues" permission granted
- For attachment migration: the target must allow creating attachments
- For remote links: the target must allow linking issues

### Step 1 — Scope

Choose what data to include **before** fetching anything. Your preferences are saved automatically and take effect when you fetch issues in the next step.

| Setting | What it does |
|---------|-------------|
| **Include all subtasks** | Automatically fetches every sub-task for matched parent issues, preserving hierarchy |
| **Include all Epic children** | Fetches all child issues (Stories, Tasks, etc.) for any Epic matched by your JQL |
| **Mentioned links as remote links** | Scans descriptions and comments for Jira URLs and URLs from domains you configure (e.g., Confluence, Figma) and creates remote links in the target |
| **Source comments in summary** | Includes up to 50 recent comments (with author and timestamp) in the migration summary comment |
| **Migrate attachments** | Uploads source attachments to target issues; falls back to remote links or comments if upload fails |
| **SCM branches and pull requests** | Collects branch/PR data from the Jira dev panel and adds it to the migration summary comment. Choose between GitHub and GitLab as the provider. **Server/Data Center only** — commits are excluded |

Click **Continue to Source** when ready.

### Step 2 — Source

Connect to the Jira instance you want to migrate **from** and fetch the issues.

1. **Capture the source instance** — open the source Jira in a browser tab, then click **Set as Source** in the extension. The extension detects the active tab's URL. A green "Verified" badge confirms the match; a "Tab Mismatch" warning means you need to switch tabs
2. **Define your JQL** — either type a query manually, or navigate to the **advanced issue search** (`/issues`) on the source instance and click **Refresh from tab** to capture the active query
3. **Fetch Issues** — click the button to download matching issues. They are cached locally. After fetching you see issue counts (parents, sub-tasks) and a preview table
4. **Estimation field discovery** — the extension scans fetched issues for numeric custom fields (e.g., Story Points). Select the correct one if prompted, or skip if estimation data is not relevant
5. **Epic link detection** — automatically detected based on project type (company-managed uses a custom field; team-managed uses the parent field)

Click **Continue to Target** once issues are cached.

### Step 3 — Target

Connect to the destination Jira instance and load its project configuration.

1. **Capture the target instance** — switch to the target Jira tab and click **Set as Target**. The same verification badge system applies
2. **Select the target project** — type or search for the project key. If the active tab is on the target instance, the project list loads automatically. The project key from your current tab may be auto-detected
3. **Load Configuration** — click the button to fetch the target project's metadata. This runs several checks in sequence:
   - Permission check (create, link, attach, comment)
   - Assignable users
   - Issue types and fields (required vs. optional)
   - Workflow statuses and transition graphs
   - Components
   - Estimation field discovery
   - Epic link configuration

   Results are shown step-by-step. Each step gets a status icon (success, warning, info, or error)

4. **Review** — after loading, you see a summary of available issue types, required/optional field counts, and assignable users. Expand "View fields" for the full list

Click **Continue to Validate** once the target is loaded.

### Step 4 — Validate & Map

Map source data to target equivalents. This is the most detailed step.

1. **Summary** — shows readiness at a glance: how many issues are ready, how many have errors, and the status of each mapping category. Use the **Auto-map all** button to let the extension match by name across all categories at once

2. **Issue Type Mapping** — assign a target issue type for every source type. Unmapped types block migration. The extension shows how many source issues use each type

3. **Field Mapping** — map source fields to target fields. Required target fields are highlighted. Each mapping row lets you pick from available target fields. Sub-configurations appear inline for fields that need value-level mapping:
   - **Priority** — map source priority values (e.g., "High") to target values
   - **Components** — map source components to target components, or mark missing ones for auto-creation
   - **Labels** — include or exclude individual labels; add extra labels
   - **Required picklists** — any required field with a fixed set of allowed values gets its own mapping panel

4. **Status Mapping** (optional) — if the target project has workflow statuses, you can enable status mapping. Map each source status to a target status per issue type. The extension uses workflow transition graphs (or live probing as fallback) to execute transitions after creation

5. **Assignee Mapping** (optional) — match source users to target users. Unmatched users are left unassigned

6. **Preview & Validate** — the final section shows every issue with its validation result. Filter by "ready", "errors", or "skipped". Toggle individual issue selection. Issues with errors cannot be migrated until the underlying mapping issue is resolved

Click **Continue to Migrate** once at least one issue passes validation.

### Step 5 — Migrate

Review the execution plan and run the migration. **Your active tab must be on the target Jira instance** while migrating.

**Before starting** you see:
- **Overview** — issue counts by category (epics, standard, sub-tasks, skipped, duplicates)
- **Execution Plan** — the order of operations: create components (if any) → create epics → create standard issues → create sub-tasks → transition statuses → create remote links → migrate attachments → enrich with comments and assignee updates
- **Options Summary** — all active settings (batch size, skip-existing, labels, source comments, status mapping, assignee mapping, attachments, SCM data)
- **Not Migrated** — a reminder of what Jira controls (timestamps, reporter, resolution) and what the extension doesn't recreate as native links

**During migration:**
- A live progress bar and issue-by-issue log update in real time
- Each phase gets a step indicator (pending → active → success/warning/error)
- You can **Stop Migration** at any time; already-created issues remain in the target

**After completion** you see a results summary:
- Created / failed / skipped counts and success rate
- Detailed breakdowns for status transitions, remote links, attachments, and enrichment
- Expandable lists of created issues (source → target key), failures with error messages, and per-category failures
- **Copy CSV** to export `sourceKey,targetKey,status,error` for all processed issues
- **Open in Jira Search** to view created issues in the target
- **Start New Migration** to reset and begin again

# authentication & privacy

**No API tokens or credentials required.** The extension piggybacks on your existing Jira session.

When you're logged into Jira in your browser, the extension can make requests to the Jira API on your behalf using the same session cookies. This works because of the browser's same-origin policy - requests from the extension to your Jira domain automatically include your authentication.

**What this means for you:**
- No setup or configuration needed
- No API tokens to manage or rotate
- Your credentials are never stored by the extension
- Access is limited to what your Jira account can already see

**Data storage:**
- Datasets and analysis results are stored in your browser's local storage (IndexedDB)
- Nothing is sent to external servers
- Clearing your browser data removes all extension data
- You can clear extension storage anytime via the control panel