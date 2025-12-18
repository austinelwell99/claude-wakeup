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

## Viewing Logs

**Actions** tab → Click any workflow run → **wakeup** → **Ping Claude API**

## Schedule

Runs daily at 6:05 AM US Central Time.

Note: GitHub uses UTC, so during Daylight Saving Time (March-November) it will run at 7:05 AM CDT instead. To adjust, edit the cron in `.github/workflows/claude-wakeup.yml`:
- Winter (CST): `5 12 * * *`
- Summer (CDT): `5 11 * * *`

## Troubleshooting

- **Workflow not running?** GitHub cron can be delayed up to 15 min. Also, workflows pause after 60 days of repo inactivity - just re-enable in Actions tab.
- **API errors?** Verify your `ANTHROPIC_API_KEY` secret is set correctly.
