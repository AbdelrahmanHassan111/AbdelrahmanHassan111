# Setup Guide — everything needed for it to actually work

## File layout

```
AbdelrahmanHassan111/
├── README.md
├── assets/
│   ├── header.svg
│   └── terminal.svg
└── .github/
    └── workflows/
        ├── metrics.yml
        ├── snake.yml
        └── activity.yml
```

## Step 1 — Push the files

Copy these files into your cloned repo exactly in the layout above, commit, push to `main`.

The **header banner**, **terminal card**, and **joke card** work immediately — no setup needed, they're either plain files in your repo or a public API.

## Step 2 — Create the METRICS_TOKEN secret

1. `github.com/settings/tokens/new` → classic token
2. Check scopes: `repo` and `read:user`
3. Generate → copy the token
4. Repo → Settings → Secrets and variables → Actions → New repository secret
5. Name exactly `METRICS_TOKEN`, paste value, save

## Step 3 — Run each workflow once

Repo → **Actions** tab → for each of "Metrics Dashboard", "Generate Snake", "Update Recent Activity": click it → **Run workflow** → wait for the green checkmark.

## Step 4 — Confirm

Refresh `github.com/AbdelrahmanHassan111`. All three dynamic pieces should now show.

---

## Troubleshooting — what actually went wrong last time

**"Live Dashboard" section was empty / broken image:**
This means `metrics.svg` was never created in your repo, which happens if either:
- The `METRICS_TOKEN` secret was never added, or its name doesn't match exactly, **or**
- The workflow ran but failed. **Go check**: Actions tab → click on the most recent "Metrics Dashboard" run → if there's a red ❌, click into it and read the error. The two most common causes:
  - `Bad credentials` → the token is wrong/expired → regenerate it and update the secret
  - `Resource not accessible` → the token is missing the `read:user` scope → regenerate with both scopes checked

I've also simplified the workflow this time — a couple of the plugin options I used before weren't confirmed-safe and could have caused the whole run to fail with a config error before it even got to your data. The new version only uses options directly confirmed from the tool's own documentation.

**"Recent Activity" section was empty:**
The `activity.yml` workflow needs to actually run at least once (it fills the space between the `<!--START_SECTION:activity-->` and `<!--END_SECTION:activity-->` comments in your README). If you added the workflow file *after* already running it, or never triggered it manually, that's why it was blank. Run it once from the Actions tab.

**A stray `-->` was visible as plain text on the page:**
That was a bug in my previous README — a malformed HTML comment block for an optional WakaTime section. I've removed it completely in this version; WakaTime setup instructions live only here in `SETUP.md` (see below) so they can never leak into the visible page again.

**General rule:** if any image looks broken, right-click it → "Open image in new tab". A JSON error text means a bad/missing token. A 404 means the workflow hasn't run yet or the branch it needs (`output`, for the snake) doesn't exist yet.

---

## Optional — WakaTime coding-time stats

Only worth doing if you actually use WakaTime day-to-day.

1. Create an account at wakatime.com, install its plugin for your editor(s)
2. Code for at least a day so it has data
3. Get your API key from wakatime.com/settings/api-key
4. Add it as a repo secret named `WAKATIME_API_KEY`
5. Add `waka.yml.optional` to `.github/workflows/` and rename it to `waka.yml`
6. Add a "Weekly Coding Activity" section to your README with:
   ```
   <!--START_SECTION:waka-->
   <!--END_SECTION:waka-->
   ```
7. Run the new workflow once from the Actions tab
