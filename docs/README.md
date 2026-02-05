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

Create multiple sub-tasks for any Jira issue in one go. Navigate to a Jira issue page, open the extension side panel, and enter multiple sub-task titles (one per line) to create them all at once.

**Requirements:**
- Active Jira issue page (the extension detects the issue from the URL)
- The issue type must support sub-tasks (e.g., Stories, Tasks)
- Project must have sub-tasks enabled

**How it works:**
1. Open any Jira issue in your browser
2. Open the extension side panel
3. Navigate to "Bulk Sub-tasks"
4. Enter sub-task titles (one per line)
5. Click submit to create all sub-tasks

## Epic Audit

Review epic health, completion status, and child issues across a project or custom JQL query. Get an overview of your epics grouped by status and a detailed breakdown of each epic's children.

**Requirements:**
- Active Jira project page or a saved JQL query from advanced search

**How it works:**
1. Navigate to a Jira project or run an advanced search
2. Open the extension and go to "Epic Audit"
3. Click "Audit project" or "Audit via JQL" to analyze epics
4. View grouped summaries and detailed breakdowns of epic children and their completion status

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

## Migrate Issues

Copy issues between different Jira instances. A step-by-step wizard guides you through selecting source issues, configuring the target instance, validating the migration, and completing the transfer.

**Requirements:**
- Access to both source and target Jira instances
- Appropriate permissions to read from source and create issues in target

**How it works:**
1. **Source Step**: Select the issues you want to migrate from your current Jira instance
2. **Target Step**: Configure the destination Jira instance and project
3. **Validate Step**: Review the migration mapping and resolve any field conflicts
4. **Post Step**: Execute the migration
5. **Complete Step**: View the results and links to newly created issues

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