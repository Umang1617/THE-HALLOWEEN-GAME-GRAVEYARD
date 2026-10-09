# Halloween Games Graveyard - deploy guide

Files
- `index.html`  the whole site (one file)
- `apps-script/Code.gs`  the backend that writes votes into a Google Sheet
- `vercel.json`  optional Vercel settings

## Step 1 - Google Sheet backend (5 minutes, do this first)
1. Create a new Google Sheet (name it e.g. "Halloween Votes").
2. Extensions > Apps Script. Delete the sample code, paste all of `apps-script/Code.gs`, save.
3. Pick `setup` in the function dropdown and click Run. Approve the permissions
   (Google shows an "unverified app" notice because it is your own script: Advanced > Go to project).
   This creates three tabs: **Voters**, **Votes**, **Game tallies**.
4. Deploy > New deployment > type **Web app**. Execute as: **Me**. Who has access: **Anyone**. Deploy.
5. Copy the Web app URL (ends in `/exec`).
6. Open `index.html`, search for `SHEETS_URL: ""` and paste the URL between the quotes. Save.

Whenever you edit Code.gs later: Deploy > Manage deployments > edit > New version.

## Step 2A - Vercel
- Easiest: put this folder in a GitHub repo, then vercel.com > Add New > Project > import it.
  Framework Preset: **Other**. Leave build command and output directory empty. Deploy.
- Or with the CLI: `npx vercel --prod` inside this folder.

## Step 2B - Lovable
Lovable builds React/Vite apps, and this site is plain HTML, so it replaces the starter page:
1. Create a new empty Lovable project.
2. Open the code view (or connect GitHub), replace the root `index.html` with this `index.html`
   (it has no build step and needs no React).
3. Or paste this into Lovable's chat after uploading `index.html`:
   "Replace the root index.html with the attached file exactly as is. Do not add React,
   Tailwind or any other framework, and do not edit its contents. Remove the src/main.tsx script tag."
4. Publish.

## Where your data lands
- **Voters**: one row per person (email, Discord ID, submissions, picks per category, write-ins).
- **Votes**: one row per vote. Easy to filter or pivot.
- **Game tallies**: live vote count per game (formula, updates itself).
Download as Excel: File > Download > Microsoft Excel (.xlsx).

## Good to know
- Rules are checked on the server too: max 5 picks per category, one change per email.
- Anyone can open the Web app URL and see the anonymous vote tallies (no emails), which is how Live Results works.
  Keep the Sheet itself private (do not share it publicly).
- Voters are identified by email, so "one change" cannot be dodged by clearing the browser.
- If you leave `SHEETS_URL` empty the site still works but votes stay on each visitor's device only.
