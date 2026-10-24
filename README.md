# Goal Tracker

A single HTML file goal tracker. No server, no build step, no dependencies.

## Run

Double-click `index.html` (or drag it into Chrome). Bookmark it for quick access.

## Data

All data is saved in the browser's localStorage (key `goal-tracker-v1`), so keep
using the same browser. Clearing the browser's site data erases it.
Use ⋯ → "Export JSON" for backups; "Import JSON" restores a backup and replaces
all current data.

## Publish later (installable via GitHub Pages)

The folder already includes `manifest.webmanifest`, `sw.js` and `icons/` — they
are ignored on `file://` and activate automatically when served over https.

1. Push this folder to a GitHub repo.
2. Repo → Settings → Pages → deploy from branch `main`, root folder.
3. Open `https://<user>.github.io/<repo>/` in Chrome.
4. Install via the address-bar icon or the ⋯ menu → "Install app".

Note: data stored while using the file version and data stored on the hosted
version are separate (different origins). Move it with Export → Import.
