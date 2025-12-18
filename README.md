# Claude Wakeup - Daily API Ping

Automatically pings the Claude API at 6:05 AM Central Time daily to start your 5-hour usage window.

## Setup Instructions

### 1. Create a GitHub Repository

1. Go to https://github.com/new
2. Name it something like `claude-wakeup`
3. Select **Private** (recommended, since this involves your API usage)
4. Click **Create repository**

### 2. Add Your Anthropic API Key as a Secret

1. In your new repo, go to **Settings** (tab at the top)
2. In the left sidebar, click **Secrets and variables** → **Actions**
3. Click **New repository secret**
4. Fill in:
   - **Name:** `ANTHROPIC_API_KEY`
   - **Secret:** Your Anthropic API key (starts with `sk-ant-...`)
5. Click **Add secret**

### 3. Upload the Workflow File

#### Option A: Using GitHub Web UI

1. In your repo, click **Add file** → **Create new file**
2. In the filename field, type: `.github/workflows/claude-wakeup.yml`
   - This will automatically create the folders
3. Copy the entire contents of `.github/workflows/claude-wakeup.yml` from this folder
4. Paste it into the editor
5. Click **Commit changes**

#### Option B: Using Git Command Line

From this folder (`claude-wakeup`), run:

```bash
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/claude-wakeup.git
git push -u origin main
```

### 4. Test It

1. Go to your repo on GitHub
2. Click the **Actions** tab
3. Click on **Claude Wakeup** in the left sidebar
4. Click **Run workflow** → **Run workflow** (green button)
5. Wait ~10 seconds, then click on the running workflow to see logs

## Where to Find Execution Logs

1. Go to your repo on GitHub
2. Click the **Actions** tab
3. You'll see a list of all workflow runs with their status:
   - Green checkmark = success
   - Red X = failure
4. Click on any run to see details
5. Click on the **wakeup** job, then **Ping Claude API** to see the full output

## Schedule Details

- **Current schedule:** 6:05 AM Central Standard Time (CST)
- **Cron expression:** `5 12 * * *` (12:05 PM UTC)

### Daylight Saving Time

GitHub Actions uses UTC only and doesn't adjust for DST automatically.

| Season | Time Zone | UTC Offset | Local Time |
|--------|-----------|------------|------------|
| Winter (Nov-Mar) | CST | UTC-6 | 6:05 AM |
| Summer (Mar-Nov) | CDT | UTC-5 | 7:05 AM |

**To always run at 6:05 AM local time**, you'll need to update the cron twice a year:
- **March (start of DST):** Change to `5 11 * * *`
- **November (end of DST):** Change to `5 12 * * *`

Or just leave it and accept the 1-hour shift during summer.

## Troubleshooting

### Workflow not running?

- GitHub Actions cron jobs can be delayed by up to 15-20 minutes during high-demand periods
- Workflows are disabled after 60 days of repo inactivity. Just visit the Actions tab and re-enable if needed.

### API errors?

- Check that your `ANTHROPIC_API_KEY` secret is set correctly
- Verify your API key is valid and has available credits

## Cost

- **GitHub Actions:** Free (private repos get 2,000 minutes/month; this uses ~0.1 min/day)
- **Claude API:** Minimal (a "hi" message costs a fraction of a cent)
