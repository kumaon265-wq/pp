README - Guest Room Status (No external DB)
-------------------------------------------
WHAT:
- Static viewer (index.html) reads /data.json every 3 seconds.
- Admin (admin.html) updates rooms and sends updated JSON to a Netlify function.
- Netlify function updates data.json in the site's GitHub repo using GitHub API.

SETUP STEPS:
1) Create a new GitHub repository (public or private) and push all files from this package to the repository root.
2) In the repo ensure 'data.json' exists (already included).
3) Create a GitHub Personal Access Token:
   - Go to GitHub Settings -> Developer settings -> Personal access tokens -> Tokens (classic) -> Generate new token.
   - Give 'repo' scope (full repo) to allow file updates. Copy the token.
4) Deploy on Netlify:
   - Netlify -> New Site from Git -> Connect your GitHub repo -> Deploy.
5) In Netlify dashboard -> Site settings -> Build & deploy -> Environment -> Edit variables, add:
   - GITHUB_TOKEN = <your token>
   - REPO = <owner>/<repo>   (e.g. username/guest-no-db)
   - BRANCH = main  (or your branch)
   - FILE_PATH = data.json
6) In Netlify -> Functions ensure functions are enabled (Netlify will build them automatically).
7) After deploy, open /index.html for viewers and /admin.html for admin.
   - Admin will fetch current data.json, modify it, and POST to /.netlify/functions/update which will commit the updated data.json to GitHub.
   - Once committed, Netlify will redeploy the site automatically (Netlify detects changes and redeploys), making the new data.json live.

NOTES:
- This uses GitHub to store the single JSON file; no external database required.
- The GitHub token must be kept secret. Anyone with token could modify the repo.
- If you prefer not to use GitHub token, we can set up a different provider (e.g., a tiny backend) but that would be a real server.
