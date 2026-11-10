# OSU Skill Tracker (Linux)
---
A native GTK 4 / libadwaita desktop app for browsing, analysing head timing, and exporting local OSU! replays. No OSU! account connection or API token is required. 

Version `0.1.0` is the current version. It is also the first public version for release.

---
## Optional Live Tracking
---
**Settings -> Live timing -> Install tosu** downloads the *optional* Linux x86_64 helper directly from its official release. OST Verifies the pinned archive and the executable's SHA-256 before installing it under `~/.local/share/osu-skill-tracker/helpers/tosu` (or your `XDG_DATA_HOME`). The installation needs no administrator password(s). The helper is downloaded on demand, and is not included in the OST AppImage by default.

**Exact tracking (tosu)** enables the experimental live capture. It remains off by default, including after installation (unless turned on before installing initially). OST starts its installed helper when the mode is on and stops it when the mode is turned off, or OST closes. An already running external tosu is used without taking ownership of it. You can start a new play after enabling the mode. The settings show the connection status and let you change tosu's local port (default 24050). No account token is required.

Tosu reads the game's memory. If it reports Linux denying access, OST will explain the optional `CAP_SYS_PTRACE` permission before offering a desktop administrator authentication prompt (`pkexec` / `setcap`). This capability allows the helper to read other processes, not only OSU!. It is granted only after confirmation, applies to the verified helper executable, and is removed when that executable is repaired/replaced. OST does not run as root or relax system-wide practice rules. The permissions help button links to [tosu's instructions](https://github.com/tosuapp/tosu#linux-notice).

OST's managed helper has separate configuration/data/cache folders, listens on localhost, and disables its overlay, dashboard auto-opening, self-updater and PP calculator. Game updates can break its reader, so use Install/Repair to restore the pinned version if needed. No existing external helper or configuration is overwritten.

OST preserves fractional live hit errors locally under `$XDG_DATA_HOME/osu-skill-tracker/live-timing` (normally `~/.local/share/osu-skill-tracker/live-timing`).Complete captures are associated with the saved play and available in its details and JSON/CSV exports while the mode is enabled. Live results are labeled **Live UR**, ordinary reconstruction remains as **Sample UR**. Old replay files cannot recover original live precision.

The first integration accepts supported default-mod OSU!standard scores only, checks capture counts against the saved score and matching map, and falls back to replay analysis for missing, incomplete or unsupported captures. Maps with spinners and misses currently fall back because the summary cannot identify which misses were spinners. I did do some testing with a live 32-circle Double Time play, which matched lazer's displayed mean error and UR. Other maps and mods still need to be verified. The settings name describes its purpose, and is not a guarentee of universal agreement.

---

## Using the app
---
The app detects common OSU! data directories, including `~/.local/share/osu`. Open **Settings** (The top-right gear button) to select or correct the directory. Select the **data folder**, not the folder holding the OSU! AppImage.

- **Session** shows newly detected replays with play timestamps after the tracker started (The tracker starts by opening the app)
- **History** shows saved replays, newest first. Opening a historical replay does not add it to the Session
- **Settings** contains theme selection, a spot to put your player name, the OSU! data folder, an optional extra replay folder, the report destination, and an setting to set where your OSU! AppImage is at, also allowing you to launch the game inside OST. Without a player filter, replays from all players are visible, including downloaded/imported replays
- **Open replay** opens a standalone `.osr` without adding it to the session
- **Export** saves the selected replay or session as JSON, CSV, or both. The default destination is `~/osu-skill-tracker-exports/`, you can change this in **Settings**

#### Extra notes:
- Choosing a theme applies the theme immediately, and is remembered across app restarts
- The replay view uses a compact score summary and judgement breakdown. File hashes and paths are under **More details**
- For Lazer, the app reads replay files directly from its hashed `files/` store, so manual replay exporting should normally be unnecessary. It also watches `exports/` and stable-style `Replays/` / `replays/` folders. Exporting to `/.local/share/osu/exports/` remains as a fallback
- Detection polls every three seconds and waits for a new file to remain unchanged across scans before parsing it, typical detection takes three to six seconds
- Local beatmap files supply map title, artist, difficulty name, and mapper through exact MD5 matching
- All OSU! data is read-only. **The tracker does not open or modify `client.realm`**.
- **The app never searches your whole computer**.
- **OST does not inject code into OSU! or modify the game. Optional exact tracking uses tosu, a separate helper that reads the running game's memory to obtain precise timing data. Tosu is off by default.**

---
## Versions and updates
---
You can see what version you have installed by looking at the bottom right corner, and under **Settings -> Application**. The app looks for new builds on this repository on startup and every six hours if automatic checking is enabled via Settings.

If the app fails to update for whatever reason, it will restore a temporary backup it makes before downloading the update to easily revert back to your working version.

When the app updates, it also publishes a new `update.json` beside the AppImage and checksum. It contains the release version, size, SHA-256, download URL, and changes

---
## Bug reports
---
Bug reports are done inside of this repo's [Issues](https://github.com/Ali-Hammond/OSU-Skill-Tracker-Linux-Releases/issues/) tab. For a quick link, you can open **Settings -> Help -> Report a bug**. Clicking the link inside of the app is the best practice, as it will give you a pre-filled form to work from, instead of starting from scratch. A GitHub account is required. Make sure your report has the following before submission:
- What happened
- How to reporoduce
- Expected result
- App version
- Linux Distribution
- Architecture
- Kernal version
- Session (X11, wayland, etc)
- GTK version
- libadwaita version
- Renderer
- libc version
- Screenshots or any other details

Avoid giving any personal information. If you click the bug report button inside of the app, 90% of the list above will already be filled out for you. You are 100% free to remove any information you do not wish to share, but please keep in mind that the more I know about what your environment has/is running, the more I will be able to tune it to work correctly for your specific case.

If you have any questions or concerns, please open a GitHub Issue.