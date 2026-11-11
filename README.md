# Goal Tracker

A single HTML file goal tracker. No build step or application server is required.

## Run locally

From the repository directory, start a small static web server:

```bash
python3 -m http.server 8000
```

Open <http://localhost:8000/> in your browser.

Using localhost is recommended for testing cloud sync. The app also works offline for normal tracker use; cloud sync requires an internet connection.

## Data and backups

Tracker data is saved in browser localStorage under `goal-tracker-v1`. Use ⋯ → **Export JSON** to create a backup and **Import JSON** to restore one. Import replaces the data currently open in that browser.

Keep a separate copy of your JSON export before the first cloud upload.

## Supabase cloud sync

The app uses Supabase Auth and the `public.goal_tracker_data` table. The browser contains only the Supabase project URL and publishable key; never put a Supabase secret/service-role key in this repository.

The table must have Row Level Security enabled and policies that restrict select, insert and update to rows where `auth.uid() = user_id`. The expected table shape is:

- `user_id` UUID primary key referencing `auth.users(id)`
- `data` JSONB
- `version` BIGINT
- `updated_at` TIMESTAMPTZ

### Configure redirect URLs

In Supabase → Authentication → URL Configuration, allow the URLs you use to sign in, including:

- Your deployed Goal Tracker URL: `https://simplyjaga.github.io/goaltracker/`
- Local testing: `http://localhost:8000/`

Keep the deployed URL as the Site URL if that is your production app.

### First upload

1. Open the app and use ⋯ → **Import JSON** to import your existing backup.
2. Check that your goals, plans, tasks and habits appear correctly.
3. Choose ⋯ → **Cloud sync…** and enter the email address for your Supabase account.
4. Open the sign-in email in the same browser. Return to the tracker and choose **Cloud sync…** again.
5. If no cloud backup exists, confirm **first upload**. The app uploads the current tracker state; it does not modify your JSON file.
6. Verify the row in Supabase Table Editor → `goal_tracker_data`.

### Syncing another device

Sign in with the same email on the other device and choose **Cloud sync…**. If cloud data exists, type **DOWNLOAD** to restore it to that browser, or **UPLOAD** only when you intentionally want that browser's data to replace the cloud copy. Export a JSON backup before replacing local or cloud data.

The app does not silently synchronize changes in the background. It uses explicit actions and a version check to avoid overwriting a cloud update that happened after it was read. Keep exporting JSON backups as an independent recovery option.

## GitHub Pages

In the repository, open Settings → Pages and deploy from the `main` branch, root folder. The project site is <https://simplyjaga.github.io/goaltracker/>.
