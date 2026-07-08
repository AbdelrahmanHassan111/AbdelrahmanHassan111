# Setup Guide — everything needed for it to actually work

Total time: ~15 minutes, one-time. After this, everything updates itself automatically forever.

Your profile repo must be named **exactly** `AbdelrahmanHassan111` (public), with this `README.md` on the `main` branch. If you haven't made it yet: create a new **public** repo named `AbdelrahmanHassan111` on GitHub — the special profile-README behavior only activates when the repo name matches your username exactly.

## File layout

Copy the files from this delivery into your repo so it looks like this:

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

(`waka.yml.optional` stays out of `.github/workflows/` unless you do step 6 below — a `.optional` file is ignored by GitHub Actions.)

## Step 1 — Push the base files

Add `README.md`, `assets/header.svg`, `assets/terminal.svg`, and the three `.yml` files above into `.github/workflows/`. Commit and push to `main`.

At this point your **header banner and terminal card already work** — they're plain SVGs rendered from your own repo, no setup needed beyond being pushed.

## Step 2 — Create a Personal Access Token (needed only for the metrics dashboard)

`metrics.yml` needs to read things the default token can't (stars, languages, full account activity).

1. Go to **github.com/settings/tokens/new** → classic token.
2. Scopes: check `repo` and `read:user`.
3. Generate → copy the token (you won't see it again).
4. In your profile repo: **Settings → Secrets and variables → Actions → New repository secret**.
5. Name it exactly `METRICS_TOKEN`, paste the value, save.

## Step 3 — Run the workflows once each

Go to your repo's **Actions** tab. GitHub sometimes asks you to click "I understand my workflows, enable them" the first time — do that, then:

1. Select **Metrics Dashboard** → **Run workflow** → wait ~1–2 minutes. It commits `metrics.svg` to your repo root automatically.
2. Select **Generate Snake** → **Run workflow** → wait ~1 minute. It creates a new `output` branch automatically and pushes the snake SVGs there.
3. Select **Update Recent Activity** → **Run workflow** → wait a few seconds. It fills in the `<!--START_SECTION:activity--> ... <!--END_SECTION:activity-->` block in your README with your last 5 public GitHub events.

## Step 4 — Confirm it worked

Refresh your profile page (`github.com/AbdelrahmanHassan111`):
- **Header banner + terminal card** → visible immediately after Step 1.
- **`metrics.svg` dashboard** → visible after Step 3.1. If it's a broken image icon, re-check the `METRICS_TOKEN` secret name and scopes.
- **Snake animation** → visible after Step 3.2 *and* after the `output` branch exists — check the repo's branch dropdown to confirm `output` was created.
- **Recent Activity list** → visible after Step 3.3, and updates every 2 hours automatically after that.

## Step 5 — Ongoing maintenance

None. All four workflows are on schedules (`metrics`: daily, `snake`: every 6 hours, `activity`: every 2 hours) and will keep rebuilding themselves. You only touch this again if you want to change the design.

## Step 6 — Optional: real coding-time stats (WakaTime)

Only do this if you actually want it — it requires you to use WakaTime day-to-day, otherwise the card will just say "no data."

1. Create a free account at **wakatime.com** and install its plugin for your editor(s) (VS Code, PyCharm, etc.).
2. Code for at least a day so it has data.
3. Get your API key from **wakatime.com/settings/api-key**.
4. Add it as a repo secret named `WAKATIME_API_KEY`.
5. Rename `waka.yml.optional` → `waka.yml` and move it into `.github/workflows/`.
6. In `README.md`, delete the `<!--` and `-->` lines around the "Weekly Coding Activity" block so it's no longer commented out.
7. Run the new workflow once from the Actions tab.

## Troubleshooting

- **An image shows as a broken icon**: right-click → open image in new tab. If it shows a JSON error, the secret/token is wrong or missing. If it's a 404, the workflow hasn't run yet or the `output` branch doesn't exist yet — run it from the Actions tab.
- **Nothing updates**: check the Actions tab for a red X on a run — click it to see the actual error log.
- **Everything renders but looks unstyled/plain**: you're probably viewing the raw file on `raw.githubusercontent.com` instead of your rendered profile page at `github.com/AbdelrahmanHassan111` — check the actual profile page.
