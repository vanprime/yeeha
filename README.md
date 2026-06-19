# Yeeha

Yeeha is a browser extension that extends Jira without paid plugins or external services.
It runs against your existing Jira session and keeps product data in local browser storage.

## Installation

### Chrome

1. Download the latest [release](https://github.com/vanprime/yeeha/releases).
2. Open `chrome://extensions/` and enable Developer mode.
3. Click Load unpacked and select the release folder.
4. Pin the extension and open the side panel.

<img src="./images/chrome-developer-mode.png" width="300">
<img src="./images/chrome-load-unpacked.png" width="300">
<img src="./images/chrome-pin-extension.png" width="300">

## Data Privacy

No API tokens or credentials are required. The extension uses your existing Jira session
cookies with `credentials: "include"`, and there is no analytics or telemetry pipeline.

### Network Calls

The extension communicates with:

| Destination | Methods | What for |
| --- | --- | --- |
| Your source Jira instance | `GET` | Read issues, comments, links, projects, workflows, boards, and SCM metadata |
| Your target Jira instance | `GET`, `POST`, `PUT` | Create or update issues, upload attachments, transition issues, and validate target configuration |
| `api.github.com` | `GET` | Check for extension releases |

### Local Storage

| Mechanism | What it stores |
| --- | --- |
| IndexedDB `yeeha-v4-osa` | OSA audit runs, issue snapshots, workflow profiles, adjustments, and latest workitem cache |
| IndexedDB `yeeha-v4-jira-issue-storage` | Jira source-domain records and source-target mapping ledger entries |
| Legacy IndexedDB databases | Removable data from retired or archived features, including old migration and analysis databases |
| `chrome.storage.local` | Extension state, project metadata, content-script settings, OSA settings, and SCM connections |
| `localStorage` | UI preferences only |
