# GBU Campus

Instructor-produced teaching sample. This project was produced by the course instructor as a sample for learning AI-assisted development. It is not an official 3D tour of Great Bay University, an official campus map, or a surveyed reconstruction. The night story is fictional.

Game source code  
Play the game  
Try "Take the guided visit" and "Explore on foot" by day; switch to "Haunted night" for "Begin the night" and "Watch demo."

The 2026-10-05 crawling-limb revision added: articulated finger grips, uneven limb pulls, grounded contact, wet tissue, drag marks and staggered reattachment. Play Haunted night or open the Day tour. Refresh an existing tab once. References, motion review and deployment checks: `crawling-limbs-evidence.md`. Earlier character provenance remains in `realistic-students-evidence.md`, and historical Railway publication evidence in `railway-model-evidence.md`. These are historical QA records, not verification of the current Pages deployment.

One campus website with two modes: on GitHub Pages, Day tour opens at `/world/` and Haunted night at `/world/night/`. Local development uses `/` and `/night/` respectively. Use the shared header to switch in the same tab. Night pause and ending screens also offer "Return to day tour." Switching modes ends the current visit or run and stops its audio.

The rebuilt campus follows the physical model: towers at one end, the crescent and garden in the middle, and sports buildings and dormitories at the other. Day tour offers six landmark stops and a guided campus walk. Haunted night adds four location-specific challenges: restore the water reflection, listen in the west teaching block, complete a timed blue-track lap, and replay the dormitory melody. It retains GBU-shirt zombie students, blood loss, kicks, slowly reassembling limbs and touch controls; the kick now uses a photographic running shoe and trousers. Select "Watch demo" from the night title screen.

Night audio shuts down completely on victory, defeat, quit or exit. Pause, mute and leaving the game window suspend its audio engine, including during demos. Continue or Retry restores sound only for active gameplay. See `artifacts/audio-shutdown-evidence.md` for the focused checks and rollout status. Refresh an already-open game tab after an update to load the latest version.

## If you are unfamiliar with Git (new)

If you just want to download the project and run it locally, you do not need to learn every Git command first. You only need to get the source code and enter the repository root that contains `package.json` and `package-lock.json`.

### 1. Two ways to get the code

**Option A: Clone with Git (recommended, easier to update later)**

1. Install Git. On Windows, install Git for Windows; on macOS/Linux, you can usually install it via your system package manager. After installation, run in a terminal:
   
   ```sh
   git --version
   ```
   
   If it prints a version number, you are ready.

2. Clone the repository and enter the directory:
   
   ```sh
   git clone https://github.com/greenquark/gbu-campus.git
   cd gbu-campus
   ```

3. Confirm you are in the repository root. You should see `package.json`, `package-lock.json`, `tour/`, `night/`, `shared/`, and other files and folders. The later `npm ci`, `npm run build`, and `npm start` commands must all be run here.

**Option B: Download ZIP**

If you do not want to install Git for now, you can also choose "Download ZIP" on the GitHub page and enter the extracted folder. The downside is that updating later is less convenient than `git clone`; you will need to download again.

### 2. The most commonly used Git commands for first-time users

```sh
git status
```

Shows which branch you are on and which files have been modified. After a fresh clone, it usually shows you are on the `main` branch with a clean working tree.

```sh
git pull
```

If you have cloned the repository before and want to get the latest code, run this in the repository root. After updating, if dependencies have changed, re-run `npm ci`, `npm run build`, and `npm start` as described in "Run locally."

If you have modified files and want to commit:

```sh
git add <the files you modified>
git commit -m "A short description of what you changed"
git push
```

For first-time use, do not commit generated output directly. The project already states that generated `dist/` and `release/`, dependencies, local `Old/` snapshots, screenshots, recordings, and raw QA results are excluded from the Git repository. Do not add them to commits on your own.

### 3. About publishing

Committing changes to this game repository alone does **not** automatically publish a new online version. According to "GitHub Pages hosting" below, to publish a source update you need to: first merge the change into this game's `main`, then manually run the Pages workflow in the hosting repository `greenquark/greenquark.github.io`. After publishing, also check the actual workflow run and the live day/night routes.

### 4. If you are still not comfortable with Git

You can start with just three things: `git clone`, `cd gbu-campus`, and running the project as described in "Run locally." Do not rush to commit, push, or delete files. If you encounter a merge conflict, do not use force push; back up your changes first, then ask or look it up. The `node_modules` in the original development checkout may be a junction — **do not recursively delete it**; a fresh checkout creates its own dependencies with `npm ci`.

## Run locally

For a fresh checkout, install Node 22.12 or newer, open a terminal in the repository root, then run:

```sh
npm ci
npm run build
npm start
```

Open `http://127.0.0.1:5190/` for the day tour or `http://127.0.0.1:5190/night/` for the night game. Run all commands from the repository root; the root build packages both modes together. Rebuild after changing source files. The completed build works without internet access. Fonts, geometry, materials and sound generation are local.

### What to do if npm ci fails (new)

`npm ci` installs dependencies strictly according to `package-lock.json`, which makes it more suitable for this project than `npm install`. If it fails, troubleshoot in this order:

1. **Confirm your Node version**  
   Run:
   
   ```sh
   node -v
   npm -v
   ```
   
   The project requires Node 22.12 or newer. If the version is too low, upgrade Node first, then return to the repository root and retry `npm ci`.

2. **Confirm you are in the repository root**  
   You should see `package.json` and `package-lock.json` in the current directory. If you do not, `cd` to the correct directory first. On Windows you can use `dir`; on macOS/Linux use `ls` to confirm.

3. **Read the full error**  
   Re-run:
   
   ```sh
   npm ci --verbose
   ```
   
   Pay attention to the first real error, not the large stack trace at the end. Common keywords include `EBADENGINE` (Node version does not satisfy the requirement), `ENOTFOUND` / `ETIMEDOUT` (network or proxy), `EACCES` (permissions), and `EUSAGE` (lock file does not match `package.json`), among others.

4. **Check network and proxy**  
   `npm ci` needs to download dependencies. If you are behind a corporate, school, or proxy network, first confirm that npm can reach the registry. Do not manually edit `package-lock.json` just because of a network failure.

5. **Delete a normal `node_modules` and retry**  
   If this is a fresh checkout and `node_modules` is a normal folder, you can delete it and re-run `npm ci`:
   
   ```sh
   rm -rf node_modules
   npm ci
   ```
   
   On Windows PowerShell, you can use:
   
   ```powershell
   Remove-Item -Recurse -Force node_modules
   npm ci
   ```
   
   **Important**: If you are using the original development checkout and `node_modules` is a junction pointing to local Three.js/Vite dependencies, **do not recursively delete that junction**. First confirm whether it is a junction; if unsure, do not delete it — ask or use a freshly cloned repository instead.

6. **Clean the npm cache and retry**  
   You can try:
   
   ```sh
   npm cache clean --force
   npm ci
   ```
   
   This is not a universal fix, but it can help when the cache is corrupted or a download was incomplete.

7. **Do not manually edit `package-lock.json`**  
   If the error says the lock file does not match `package.json`, first confirm you did not accidentally modify `package.json`. Do not delete or hand-edit the lock file just to bypass the error; that would lose the determinism of dependency versions.

8. **Permission issues**  
   If you see `EACCES`, do not make the project files world-writable, and do not run npm with administrator privileges long-term. First check whether the project directory is under your own user directory, and whether the npm global directory permissions are normal.

9. **If it still fails**  
   Record and share: the full error message, `node -v`, `npm -v`, your operating system, whether you are in the repository root, and whether `node_modules` is a junction. That is what others need to determine whether the issue is version, network, permissions, or the lock file.

On Windows, after the first build you can also double-click `Start-GBU-Campus.cmd`. It uses the existing Codex Node runtime when available, otherwise Node from PATH, and opens the game.

5190 is the default local port. Both `npm start` and the Windows launcher respect the `PORT` environment variable. To choose another port, set it before starting. For example, in PowerShell:

```powershell
$env:PORT = "5200"
.\Start-GBU-Campus.cmd
```

The launcher starts/checks the server on that port and opens `http://127.0.0.1:5200/`. Night mode is at `http://127.0.0.1:5200/night/`. You can also run `npm start` instead of the launcher after setting the same variable, then open the matching URL yourself. Use a whole-number port from 1 to 65535; if another app uses it, choose a different port. To return to the default in that PowerShell session, run `$env:PORT = $null`. This local port setting does not change the GitHub Pages URLs.

The original development checkout uses a junction to local Three.js/Vite dependencies; do not delete that `node_modules` junction recursively. Fresh checkouts create their own dependencies with `npm ci`.

The Git repository includes `tour/`, `night/`, `shared/`, source assets, build/server tools and written evidence. Generated `dist/` and `release/`, dependencies, local `Old/` snapshots, screenshots, recordings and raw QA results are excluded. Evidence documents describe historical runs; their local capture paths are not included in this source repository.

## GitHub Pages hosting

Production hosting is GitHub Pages, at `https://greenquark.github.io/world/`; night mode is at `https://greenquark.github.io/world/night/`.

The Pages workflow lives in the hosting repository, `greenquark/greenquark.github.io`. It checks out this game repository, adapts asset/navigation/build paths to the `/world/` prefix, installs dependencies with Node 22, builds both scenes and copies the static `dist/` output into `public/world/`. It uploads the assembled static site and deploys it to GitHub Pages.

To publish a source update, merge it into this game's `main`, then manually run that workflow in the hosting repository. The current workflow also runs on changes to the hosting repository's `main`; a commit to this game repository alone does not trigger it. Check the actual workflow run and the live day/night routes after publishing. The workflow is the authority for the current packaging steps; this README change does not itself certify a fresh deployment or playtest.

Pages serves the static build. The local `npm start` server and the `release/` Node/Docker package are not the Pages hosting payload. The older Railway server/health-check instructions and deployment evidence remain in Git history and the historical evidence documents; Railway is not the current hosting route.

Both daytime and night scenes have independent route bundles, so one mode is active at a time. Debug scene hooks are available only with `?debug`; normal visitors see no test interface.

## Verification and provenance

For the 2026-10-02 rebuild, see `model-rebuild-evidence.md`: 26 desktop/phone captures, complete input-driven and live-browser wins, final map/contact/camera checks, duplicate-image audit and exact publication status. The map pauses gameplay and sound until closed. Physical-phone testing remains outstanding; phone controls were verified through browser touch emulation.

The earlier integration suite uses local Chrome through Playwright, checks same-tab navigation, outgoing audio disposal, public routes, bite/kick behavior, demo switching, modal keyboard access, browser Back behavior and emulated touch navigation. Eight desktop/phone captures cover both title and active states of that earlier release. See `artifacts/final-evidence.md` for its historical results and deployment status. QA tooling additionally needs Playwright and pngjs; this device uses the Codex dependency runtime through `NODE_PATH`.

Relative building locations follow the supplied physical-model photos/video; visible facades follow the organized campus photos. Distances are compressed for gameplay. Unseen elevations, interiors and the night story remain interpretations. The site is an independent study, not an official or surveyed university reconstruction.

The original `Refined_Tour` and `Haunted_Campus` folders are preserved as source history. Future changes to the unified website belong in the `Campus_Experience` folder. Three.js and Vite are MIT-licensed; font licenses remain with their assets. CC0 environment/material sources are retained under `tour/public/environment/SOURCES.md`. The existing GBU shirt crest is retained unchanged.
