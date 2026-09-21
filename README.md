# PesaTrack remote update config

Public, read-only JSON used by the PesaTrack mobile app for optional and forced
update prompts. No secrets — safe to keep this repository **public**.

The app fetches:

`https://raw.githubusercontent.com/AK1239/pesatrack-remote-config/main/update_config.json`

## Schema

Edit `update_config.json`:

| Field | Meaning |
| --- | --- |
| `minVersion` | Installed version below this → forced update |
| `latestVersion` | Installed version below this → optional update (if not forced) |
| `forceUpdate` | If `true`, forced update for everyone on that platform |
| `updateUrl` | Play Store / App Store link |
| `message` | Dialog body text |

After each store release, bump `latestVersion` to match `pubspec.yaml` (the part
before `+`). Use `minVersion` or `forceUpdate` only when you need a hard block.

## One-time setup (new public GitHub repo)

From this folder (`pesatrack-remote-config`, **outside** the private app repo):

```bash
cd "/Users/akil/Projects/Finance Tracker/pesatrack-remote-config"

git init
git add update_config.json README.md
git commit -m "Add remote update config for PesaTrack"

git branch -M main
git remote add origin https://github.com/AK1239/pesatrack-remote-config.git
git push -u origin main
```

On GitHub first:

1. **New repository** → name: `pesatrack-remote-config`
2. Visibility: **Public**
3. Do **not** add a README, `.gitignore`, or license (this folder already has them)

Verify in a private browser window:

`https://raw.githubusercontent.com/AK1239/pesatrack-remote-config/main/update_config.json`

You should see JSON, not `404`.

## Day-to-day (each release)

```bash
cd "/Users/akil/Projects/Finance Tracker/pesatrack-remote-config"
# edit update_config.json
git add update_config.json
git commit -m "Bump Android latestVersion to 1.1.6"
git push
```

Changes are live within seconds — no app rebuild required **after** the app
already points at this URL (see `docs/force-update.md` in the private repo).

## Why this lives outside the app repo

The app repo is private; GitHub raw URLs only work without auth for **public**
repositories. Keeping config here avoids nested git repos and a second source of
truth inside the private tree.
