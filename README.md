# release-assets
This repository is used to manage assets related to releases in github/releases

## Copilot Issue Slack Notifier

The workflow at `.github/workflows/copilot-issue-slack-notifier.yml` automatically sends a Slack notification to **`#support-squad-copilot-shiftlog`** whenever a new issue is opened in this repository that mentions the word **`copilot`** (case-insensitive, whole-word match) in its title or body.

### How it works

1. Triggers on the `issues.opened` event.
2. Checks the issue title and body for the whole word `copilot` (e.g. matches *"Copilot bug"* but not *"github-copilots"*).
3. If matched, POSTs a Slack message containing:
   - The fixed text `Review release and assign SME`
   - Issue title (as a link), author login, and repository name
4. If no match is found, the workflow exits silently — no notification is sent.
5. An empty or missing issue body is handled safely (treated as an empty string).

### Secret configuration

Add the following secret to this repository before the workflow can send notifications:

| Secret name         | Description                                                  |
|---------------------|--------------------------------------------------------------|
| `SLACK_WEBHOOK_URL` | Incoming Webhook URL for the `#support-squad-copilot-shiftlog` Slack channel |

To add the secret: **Repository Settings → Secrets and variables → Actions → New repository secret**.

### Quick test plan

| Scenario | Issue title | Issue body | Expected result |
|----------|-------------|------------|-----------------|
| Match in title | `Copilot is slow today` | *(any)* | Slack notification sent |
| Match in body | `Performance issue` | `Need copilot assistance` | Slack notification sent |
| Case-insensitive match | `COPILOT integration` | *(any)* | Slack notification sent |
| No match | `Release checklist` | `Deploy new version` | No notification (silent skip) |
| Partial word — no match | `github-copilot` | *(empty)* | No notification (whole-word only) |
| Empty body | `Copilot question` | *(empty/null)* | Slack notification sent (title matched) |
