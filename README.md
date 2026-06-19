Oosten apps — static frontends (GitHub Pages)
Two browser apps that load from GitHub Pages (a stable, account-agnostic URL) and
talk to their Google Apps Script `/exec` API via `fetch`. Data still lives in
Google Sheets. This fixes the "site not available / pick an account" errors you got
loading the apps directly from `script.google.com`.
```
todoosten/   → ToDoosten  (tasks)      → its Apps Script /exec
oostenbuy/   → OO$TEN     (shopping…)  → its Apps Script /exec
```
(Chicagoosten is NOT here — it has no webpage, it's just a Claude connector.)
One-time: publish on GitHub Pages
Create a free GitHub account → New repository → name it e.g. `oosten-apps`
→ Public → Create. (Do this as its OWN repo, not inside DocDouble.)
Upload this folder's contents (the `todoosten/` and `oostenbuy/` folders) to the
repo — drag-drop into Add file → Upload files → Commit.
Repo Settings → Pages → Source: Deploy from a branch → Branch: main
`/ (root)` → Save.
Wait ~1 min. Your URLs:
`https://YOURNAME.github.io/oosten-apps/todoosten/`
`https://YOURNAME.github.io/oosten-apps/oostenbuy/`
Open each on phone/desktop → Add to Home Screen. Installs as a real PWA
(works offline after first load; icons included).
Requirements on the Apps Script side (already set)
Each web app deployed with Execute as: Me, Who has access: Anyone
(anonymous — this is what lets the page call it with no login).
The `/exec` URLs are baked into each `index.html` as `EXEC_URL`. If you ever
redeploy to a NEW url (you shouldn't — use Manage deployments → New version),
update `EXEC_URL` near the top of the `<script>` and re-commit.
Password
Currently off (`REQUIRE_PASS:false` server-side; the static page sends an empty
password). To turn it back on: set `REQUIRE_PASS:true` in the app's `Code.gs`,
redeploy, then open the site → Settings → enter the password (it's stored on the
device and sent with each request).
Offline
The page loads offline via a service worker (cached shell).
Edits made offline queue in the browser and sync when you're back online.
You can also add offline straight in the Google Sheet's Inbox tab
(Col A = item/task, Col B = short code) — it imports on next sync.
Notes / known trade-offs
The frontend uses `fetch` with `Content-Type: text/plain` so Apps Script doesn't
trigger a CORS preflight (which it can't answer). Don't change that header.
With no password + "Anyone" access, anyone who has the `/exec` URL can read/write
the data. The URL is unguessable, but it's not private until you enable the password.
