# Ember — files that vanish

A single-page file-share tool. Drop a file, get a link, anyone with that link
can grab the file — then it's gone in 10 minutes.

## How it works

- Uploads go straight from the visitor's browser to **file.io** (a free,
  no-account file-relay service). Nothing is stored in this repo or on GitHub.
- The share link this page generates encodes the upload's expiry time
  directly in the URL (after the `#`). When someone opens that link, the page
  checks the time and shows a live countdown, then blocks access once 10
  minutes are up.
- No server, no database, no build step — it's one HTML file, so GitHub Pages
  can serve it as-is.

## Real limits worth knowing

Because there's no account or backend involved, some things are outside this
page's control:

- **First click wins.** file.io deletes a file as soon as it's downloaded
  once. If several people click the link, only the first one actually gets
  the file — the rest will hit a dead link even if the 10-minute timer
  hasn't run out.
- **The 10-minute cutoff is enforced by this page, not by file.io.** file.io's
  own minimum expiry is measured in days, so the raw file.io link could
  technically still exist after this page starts saying "expired." Anyone
  who only has the raw file.io link (not your `#`-fragment link) bypasses the
  countdown entirely.
- **10 MB is checked in the browser**, not by file.io — it's this app's own
  limit, set to keep uploads quick.

If you outgrow these limits (want the file to truly disappear from the host
after 10 minutes, or let more than one person download it), that needs a real
backend — for example Firebase Storage + Firestore, which can enforce
expiry server-side. Happy to help set that up if you want it later.

## Deploy it

1. Create a new GitHub repo (or use an existing one).
2. Add `index.html` to the repo root (or a `docs/` folder — see below).
3. Push it, then go to **Settings → Pages**.
4. Under "Build and deployment", set **Source** to "Deploy from a branch",
   pick your branch (usually `main`) and the folder (`/root` or `/docs`).
5. Save. GitHub gives you a URL like `https://<username>.github.io/<repo>/`
   within a minute or two.

That's it — no build tools, no dependencies to install.
