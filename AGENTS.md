# BioWords (생명과학개론 단어장 & 기출문제)

A static, client-only web app for studying Introduction to Life Sciences (생명과학개론)
vocabulary and viewing past-exam PDFs. There is no backend, no auth, and no build step.
The entire app lives in `index.html` (HTML + CSS + vanilla JS). User progress (starred
words, wrong-answer notes) is stored in browser `localStorage`.

## Cursor Cloud specific instructions

### Services

There is a single "service": a static web front end. There are no dependencies to install
(no `package.json`, no lockfiles, no bundler/transpiler, no tests).

- Run (dev): serve the repo root over HTTP, e.g. `python3 -m http.server 8080`, then open
  `http://localhost:8080/index.html`. `python3` and `node`/`npx` are preinstalled.
- Build: not applicable — edit `index.html` / `voca_data.json` directly.
- Lint/Test: none configured. For a quick data sanity check, validate the JSON:
  `python3 -m json.tool voca_data.json > /dev/null`.

### Non-obvious runtime behavior (important)

- The app does NOT read the local `voca_data.json` or local PDFs. At runtime it fetches
  them from GitHub raw URLs using constants hardcoded near the top of the `<script>` in
  `index.html` (`GITHUB_USERNAME = "Jajungi"`, `GITHUB_REPO = "biowords"`,
  `GITHUB_BRANCH = "main"`). Note the fetch target repo is `Jajungi/biowords` (with an "s"),
  while this repo's git remote is `jajungi/bioword` (no "s"). Editing the local
  `voca_data.json` or PDFs will NOT change what the running app shows unless you also change
  those constants (or the data URLs) — that is a code change, not env config.
- Outbound network egress is required for the app to work as shipped: `raw.githubusercontent.com`
  (data + exam PDFs) and `cdnjs.cloudflare.com` (PDF.js 3.11.174 + its worker).
- Persistence is per-browser `localStorage` (keys `bio_stars_v2`, `bio_wrongs_v2`); there is
  no server-side state.
