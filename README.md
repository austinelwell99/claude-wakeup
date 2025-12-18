# Claude Wakeup

Automatically pings the Claude API at 6:05 AM Central Time daily to start your 5-hour usage window early.

## Setup (2 minutes)

### 1. Fork this repo

Click the **Fork** button in the top right corner.

### 2. Add your Anthropic API key

1. Get your API key from https://console.anthropic.com/settings/keys
2. In your forked repo, go to **Settings** → **Secrets and variables** → **Actions**
3. Click **New repository secret**
4. Name: `ANTHROPIC_API_KEY`
5. Value: Your API key (starts with `sk-ant-...`)
6. Click **Add secret**

### 3. Test it

1. Go to the **Actions** tab
2. Click **Claude Wakeup** in the left sidebar
3. Click **Run workflow** → **Run workflow**
4. Wait ~10 seconds, then click on the run to verify it succeeded

That's it. The workflow will now run automatically every day at 6:05 AM Central.

## Changing the Schedule

Edit `.github/workflows/claude-wakeup.yml` and change the cron line:

```yaml
- cron: '5 12 * * *'
```

### Cron Format

```
┌───────── minute (0-59)
│ ┌─────── hour (0-23) in UTC
│ │ ┌───── day of month (1-31)
│ │ │ ┌─── month (1-12)
│ │ │ │ ┌─ day of week (0-6, Sunday=0)
│ │ │ │ │
5 12 * * *  ← "At 12:05 PM UTC, every day"
```

### Converting to UTC

GitHub Actions uses UTC. To find your UTC time:
- **US Eastern**: Add 5 hours (EST) or 4 hours (EDT)
- **US Central**: Add 6 hours (CST) or 5 hours (CDT)
- **US Pacific**: Add 8 hours (PST) or 7 hours (PDT)

**Example**: To run at 7:30 AM US Eastern (EST):
- 7:30 AM + 5 hours = 12:30 PM UTC
- Cron: `30 12 * * *`

### Common Times (US Central)

| Local Time | Winter (CST) | Summer (CDT) |
|------------|--------------|--------------|
| 5:00 AM    | `0 11 * * *` | `0 10 * * *` |
| 6:00 AM    | `0 12 * * *` | `0 11 * * *` |
| 7:00 AM    | `0 13 * * *` | `0 12 * * *` |
| 8:00 AM    | `0 14 * * *` | `0 13 * * *` |

## Viewing Logs

**Actions** tab → Click any workflow run → **wakeup** → **Ping Claude API**

## Troubleshooting

- **Workflow not running?** GitHub cron can be delayed up to 15 min. Also, workflows pause after 60 days of repo inactivity - just re-enable in Actions tab.
- **API errors?** Verify your `ANTHROPIC_API_KEY` secret is set correctly.
