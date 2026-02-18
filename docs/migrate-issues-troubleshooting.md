# Migrate-Issues Troubleshooting

This guide maps onboarding blocker IDs shown in the UI to recommended fixes.

## Setup blockers

| Blocker ID | Meaning | Recommended fix |
|---|---|---|
| `setup.source-instance-missing` | Source Jira instance is not captured | Open the source Jira tab and click **Set Source** |
| `setup.source-tab-mismatch` | Active tab is not the source instance | Switch to the source Jira tab before refreshing JQL/fetching |
| `setup.source-not-on-issues-search` | Active source page is not Jira issue search | Use **Open issue search** and run the query there |
| `setup.jql-missing` | No source JQL defined | Enter JQL manually or click **Refresh from tab** |
| `setup.source-issues-missing` | Source issues were not fetched yet | Click **Fetch Issues** |

## Target blockers

| Blocker ID | Meaning | Recommended fix |
|---|---|---|
| `target.instance-missing` | Target Jira instance is not captured | Open target Jira tab and click **Set Target** |
| `target.project-missing` | Target project key is missing | Select or type a project key |
| `target.tab-mismatch` | Active tab is not target during metadata load | Use **Switch to target tab** and retry |
| `target.metadata-missing` | Target configuration not loaded | Click **Load Configuration** |

## Review blockers

| Blocker ID | Meaning | Recommended fix |
|---|---|---|
| `review.issue-types-unmapped` | Some source issue types are not mapped | Use **Auto-map all** and complete issue type mappings |
| `review.required-fields-unmapped` | Required target fields are unmapped | Map required fields in **Field Mapping** |
| `review.no-validated-issues` | No issues are currently ready | Resolve row errors in **Preview & Validate** |

## Run blockers

| Blocker ID | Meaning | Recommended fix |
|---|---|---|
| `run.target-tab-mismatch` | Active tab is not target instance | Switch to target Jira tab |
| `run.migration-in-progress` | Migration is already running | Wait for completion or stop current run |
| `run.no-ready-issues` | No validated issues selected for migration | Return to **Review & Fix** and resolve blockers |
