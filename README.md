# Claude Wakeup

Automatically pings Claude Code at 6:05 AM Central Time daily to start your 5-hour usage window early. Uses your Claude subscription (not API credits).

## Setup

### 1. Fork this repo

Click the **Fork** button in the top right corner.

### 2. Create a GitHub Personal Access Token (PAT)

The action needs a PAT with `secrets:write` permission to auto-refresh expired OAuth tokens.

1. Go to https://github.com/settings/tokens?type=beta (Fine-grained tokens)
2. Click **Generate new token**
3. Name: `claude-wakeup-secrets`
4. Expiration: Choose a long duration (e.g., 1 year)
5. Repository access: **Only select repositories** → select your forked repo
6. Permissions → **Repository permissions**:
   - **Secrets** → Read and write
7. Click **Generate token**
8. Copy the token (starts with `github_pat_...`)

### 3. Extract your Claude OAuth tokens

Run this command on your local machine (where you're logged into Claude Code):

```bash
cat ~/.claude/.credentials.json | python3 -c "
import json, sys
d = json.load(sys.stdin)['claudeAiOauth']
print('CLAUDE_ACCESS_TOKEN:', d['accessToken'][:50] + '...')
print('CLAUDE_REFRESH_TOKEN:', d['refreshToken'][:50] + '...')
print('CLAUDE_EXPIRES_AT:', d['expiresAt'])
print()
print('Full values for GitHub Secrets:')
print('---')
print('accessToken:', d['accessToken'])
print('---')
print('refreshToken:', d['refreshToken'])
print('---')
print('expiresAt:', d['expiresAt'])
"
```

### 4. Add GitHub Secrets

In your forked repo, go to **Settings** → **Secrets and variables** → **Actions** → **New repository secret**

Add these 4 secrets:

| Secret Name | Value |
|-------------|-------|
| `SECRETS_ADMIN_PAT` | Your GitHub PAT from step 2 |
| `CLAUDE_ACCESS_TOKEN` | The `accessToken` value from step 3 |
| `CLAUDE_REFRESH_TOKEN` | The `refreshToken` value from step 3 |
| `CLAUDE_EXPIRES_AT` | The `expiresAt` value from step 3 |

### 5. Test it

1. Go to the **Actions** tab
2. Click **Claude Wakeup** in the left sidebar
3. Click **Run workflow** → **Run workflow**
4. Wait for completion and verify it succeeded

The workflow will now run automatically every day at 6:05 AM Central and count against your Claude 5-hour usage window.

## How It Works

This uses the [grll/claude-code-action](https://github.com/grll/claude-code-action) fork which:
- Authenticates using your Claude OAuth tokens
- Automatically refreshes expired tokens (using your PAT to update secrets)
- Runs a minimal Claude Code interaction that counts against your subscription

## Changing the Schedule

Edit `.github/workflows/claude-wakeup.yml` and change the cron line:

```yaml
- cron: '5 12 * * *'
```

### Cron Format (UTC)

```
┌───────── minute (0-59)
│ ┌─────── hour (0-23) in UTC
│ │ ┌───── day of month (1-31)
│ │ │ ┌─── month (1-12)
│ │ │ │ ┌─ day of week (0-6, Sunday=0)
│ │ │ │ │
5 12 * * *  ← "At 12:05 PM UTC, every day"
```

### Common Times (US Central)

| Local Time | Winter (CST) | Summer (CDT) |
|------------|--------------|--------------|
| 5:00 AM    | `0 11 * * *` | `0 10 * * *` |
| 6:00 AM    | `0 12 * * *` | `0 11 * * *` |
| 7:00 AM    | `0 13 * * *` | `0 12 * * *` |
| 8:00 AM    | `0 14 * * *` | `0 13 * * *` |

## Troubleshooting

- **401/403 errors?** Your OAuth tokens may have been invalidated. Re-run step 3 to get fresh tokens.
- **Workflow not running?** GitHub cron can be delayed up to 15 min. Workflows pause after 60 days of repo inactivity.
- **Token refresh failing?** Ensure your PAT has `secrets:write` permission on the repo.

## Credits

- [grll/claude-code-action](https://github.com/grll/claude-code-action) - OAuth-enabled Claude Code Action with auto-refresh
- [Guillaume Raille's blog post](https://grll.bearblog.dev/use-claude-github-actions-with-claude-max/) - Detailed setup guide
