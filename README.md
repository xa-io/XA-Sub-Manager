<div align="center">

# XA Sub Manager

**Run multiple FINAL FANTASY XIV clients on autopilot from one window — submarine & retainer automation, multi-account visibility, 2FA logins, notifications, and an optional remote web dashboard. A single executable. No runtime, no telemetry.**

[![Download](https://img.shields.io/badge/download-aethertek.io-8b5cf6)](https://aethertek.io/applications/xa-sub-manager.html)
[![Windows](https://img.shields.io/badge/Windows-x64-0078D6)](https://aethertek.io/applications/xa-sub-manager.html)
[![Linux](https://img.shields.io/badge/Linux-Wine-FCC624)](https://aethertek.io/applications/xa-sub-manager.html)
[![License](https://img.shields.io/badge/license-Proprietary-red)](LICENSE)

</div>

---

## What is it?

XA Sub Manager watches and drives your fleet of FFXIV accounts from one compact window. It reads each account's AutoRetainer / XA Database / Lifestream data — showing every character, submarine, and retainer at a glance — and, when you turn automation on, launches, logs in (including 2FA), arranges, and closes clients on a schedule so your submarines and ventures keep cycling without you babysitting them.

It is a single self-contained **`XA Sub.exe`** — no installer, and no .NET or other runtime to install. Automation is **off by default**; nothing launches or closes until you press **Start Automation**.

### Highlights

- **Multi-account dashboard** — summary cards, an accounts list, a cross-account character list with sort + search, and a details pane (jobs, currencies, subs, retainers).
- **Submarine & retainer automation** — sequential launch → wait for the game window → arrange → keep clients cycling; closes idle clients and re-tiles what's left.
- **2FA / OTP launches** — TOTP from Windows Credential Manager or an inline secret, delivered to XIVLauncher's OTP endpoint; validates `launcherConfigV3.json` autologin first.
- **Automatic game updates** *(opt-in)* — handles XIVLauncher's "Out of date" patch prompt: closes all clients, confirms, waits out the download, then relaunches everything.
- **Per-account control** — tick/untick an account to let automation manage it or play it yourself; per-account scheduled automation hours and `max_runtime` limits.
- **Window management** — a visual multi-monitor layout editor (drag boxes, snap to edges, capture from live windows), dynamic grid placement, or a "run minimized" mode.
- **Empire View** — an animated plot of every submarine by return ETA and rank, with retainer camps and force-close timers.
- **Force-crash recovery** — frozen clients that stop processing are killed and relaunched; stuck launchers are recovered.
- **Themes & zoom** — 13 built-in dark/light/color schemes, a privacy toggle that masks names for shareable screenshots, and a **UI scale** option (100–300%) that enlarges the whole interface — window and text together — for high-resolution monitors.
- **Notifications & logging** — Discord webhook + Pushover for major issues, file logging, and crash minidumps.
- **Optional web dashboard** — token-gated browser stats, remote start/stop and launch/close, and live low-res video of each client.

> Works alongside AutoRetainer, Lifestream, and XA Database data. It **launches and arranges** game clients but never modifies your plugin data — it only reads it.

---

## Download

Grab the latest build from the official page — **https://aethertek.io/applications/xa-sub-manager.html**:

| Platform | File | Notes |
|---|---|---|
| **Windows 10/11 (x64)** | `XA-Sub-Manager-Setup-vX.YY.exe` | **Recommended.** Per-user installer (no admin), Start Menu entry, uninstaller |
| **Windows 10/11 (x64)** | `XA-Sub-Manager-vX.YY-windows.zip` | Portable — unzip anywhere and run `XA Sub.exe` |
| **Linux (via Wine, x64)** | `XA-Sub-Manager-vX.YY-linux.zip` | Same exe under Wine; see [Linux](#linux-via-wine) |

New versions are published on the same page. **Updating keeps everything you've
set up:** run the new installer over your existing install (or extract the new
zip over the old folder) — your `config.json`, window layouts, and history sit
beside the app and are never overwritten.

---

## Quick start

### Windows (installer)

1. Download and run `XA-Sub-Manager-Setup-vX.YY.exe`.
2. Accept the license agreement — the app installs per-user (no admin rights needed).
3. Launch **XA Sub Manager** from the Start Menu.

### Windows (portable)

1. Download and unzip `XA-Sub-Manager-vX.YY-windows.zip` anywhere (e.g. `C:\Games\XA Sub Manager\`).
2. Double-click **`XA Sub.exe`**.

Either way: your accounts, characters, subs, and retainers appear immediately (automation stays **off**). Review everything, then press **Start Automation** (green) when you're ready for the launch/close loop — it turns red and reads **Stop Automation** while running; click again to stop.

> Windows may show a SmartScreen prompt for a new unsigned app — choose **More info → Run anyway**.

### Linux (via Wine)

There is no separate native Linux binary — XA Sub Manager is a Windows app that runs under **Wine**, inside the same Wine prefix you already use for XIVLauncher + FFXIV.

1. Download and unzip `XA-Sub-Manager-vX.YY-linux.zip`.
2. Edit `run-linux.sh` to point `WINEPREFIX` at your game's prefix.
3. `chmod +x run-linux.sh && ./run-linux.sh`

Full notes and known limitations are in **`README-LINUX.md`** inside that package. Linux/Wine support is **experimental**: monitoring, automation, 2FA, notifications, and the web dashboard are expected to work; window-layout moves and the video stream depend on Wine's window manager.

> **Detection caveat:** the app finds running clients with Windows APIs, so it can only see clients that run **inside its own Wine prefix**. A game started by the *native* (flatpak) XIVLauncher runs outside that prefix — its subs/accounts still show, but launch/close/crash tracking won't. For full tracking, run **XIVLauncher itself under Wine, in the same prefix** as XA Sub, so the launched game is a Wine process the app can see.

### Requirements

- Windows 10/11 x64 (or a Linux Wine prefix, see above).
- [XIVLauncher](https://goatcorp.github.io/) with the relevant plugins (AutoRetainer, etc.) configured per account.
- For 2FA: XIVLauncher's "Enable XL Authenticator app/OTP macro support" turned on.
- For window resizing/layout: run FFXIV in **windowed** mode.

---

## First-run configuration

**Single account, default XIVLauncher install? No editing needed.** On first launch — with no `config.json` present — XA Sub seeds a default **`Main`** account pointing at your platform's default XIVLauncher `pluginConfigs` folder and auto-detects the right location for your OS, so your data appears on the very first run:

| Host | Default `pluginConfigs` | Default launcher |
|---|---|---|
| **Windows** | `%APPDATA%\XIVLauncher\pluginConfigs` | `…\AppData\Local\XIVLauncher\XIVLauncher.exe` |
| **Linux (Wine + XLCore)** | `~/.xlcore/pluginConfigs` → `Z:\home\<user>\.xlcore\pluginConfigs` | Flatpak XIVLauncher (`start.exe /Unix …flatpak run dev.goats.xivlauncher`) |
| **macOS (Wine + XLCore)** | `~/.xlcore/pluginConfigs` → `Z:\Users\<user>\.xlcore\pluginConfigs` | XIVLauncher app via `open` |

The app picks the first of these that exists, so an unusual layout still resolves without any config; `%VAR%` and `{user}` in paths are expanded automatically. From your `pluginConfigs` folder it derives `AutoRetainer/DefaultConfig.json`, `Lifestream/DefaultConfig.json`, and `XADatabase/xa.db` automatically.

**Adding more accounts** — open **Settings** (button in the toolbar) and either:

- **Import Settings…** — pull in an existing Auto-AutoRetainer `config_*.json` wholesale (accounts, launchers, item values, plans, rates, highlights, display options); or
- **Add** an account and fill in its `pluginConfigs` path + game launcher. Each path field has a **`...`** browse button (and you can **double-click** the field); **UNC network paths** like `\\zell\ff14\Account2\pluginConfigs` are supported.

`config.json` lives next to `XA Sub.exe`. Start from `config.json.example`. **Updating is just replacing the files** — your `config.json`, layouts, and `sublord.db` history sit in that folder and are preserved.

---

## Configuration reference

Most options are exposed in the four-column **Settings** window, with hover help on every field. Key `config.json` sections:

- **`account_locations[]`** — one entry per account: `nickname`, `pluginconfigs_path`, `enabled`, `include_submarines`, `force247uptime`, optional `enable_2fa`, `keyring_name`/`otp_secret`, `max_runtime`, and `scheduled` + `automation_hours`. The array order is the order accounts appear and launch in — reorder them with the **▲/▼** buttons in Settings.
- **`game_launchers`** — `nickname → launcher` for each account. A launcher is either a plain path string **or** an object with arguments:
  ```json
  "game_launchers": {
    "Main": "C:\\Users\\{user}\\AppData\\Local\\XIVLauncher\\XIVLauncher.exe",
    "LinuxWine": { "path": "C:\\windows\\system32\\start.exe", "args": "/Unix /usr/bin/flatpak run dev.goats.xivlauncher" }
  }
  ```
  In the account editor these are the **Game launcher path** and **Launcher arguments** fields.
- **`WINDOW_LAYOUT`** — uses `window_layout_<name>.json` next to the exe. Layouts are specific to your monitors, so none ship with the app — **build your own** in **Settings → Create / Edit Layouts…** (drag a box per account, or capture from your live windows, then Save).
- **Value maps** (`item_values`, `submarine_plans`, `build_gil_rates`, `build_consumption_rates`) — edited in `config.json` or via Import only.

**Make the UI bigger:** Settings → Display → **UI scale %** (100–300%) zooms the entire interface — window, dialogs, and text — and renders crisply (the app is DPI-aware). It applies after you restart XA Sub.

### 2FA accounts

Set `enable_2fa: true` on the account and either:

- **`keyring_name`** — the Windows Credential Manager entry holding the base32 OTP secret (preferred; keeps the secret out of the file), or
- **`otp_secret`** — the base32 secret inline (entered masked; `config.json` is locked to an owner-only ACL when saved).

XIVLauncher must have OTP macro support enabled; the app forces `OtpServerEnabled` before launching. `OTP_LAUNCH_DELAY` controls the delay between launch and code delivery.

### Manage toggle & scheduled hours

- The checkbox beside each account decides whether automation manages it. Unchecked = `(manual)`: never launched/closed/moved, but still shown. Changes persist to `config.json`.
- **Schedule (per account):** open **Settings → double-click an account** and use the **Schedule** panel. Tick **Restrict automation to set hours**, then add one or more windows (Start/End in 24-hour `HH:MM`, optional weekday toggles — leave all days unticked for *every day*; an End earlier than Start spans midnight). Outside its windows an account shows `(hours closed)` and its clients are closed.

---

## Web dashboard (remote access)

An optional embedded web server gives you a browser dashboard plus a control API — watch stats and start/stop automation, enable/disable accounts, and launch/close clients from a phone or another PC. **Off by default.**

Enable it in **Settings → Web Server** (applies live on Save) or in `config.json`:

- `WEB_SERVER_ENABLE` — start the server with the app.
- `WEB_SERVER_PORT` — listen port (default `4269`).
- `WEB_SERVER_BIND_ALL` — `false` (default) = this PC only; `true` = LAN/remote (**requires a token**).
- `WEB_SERVER_TOKEN` — shared secret required on every `/api/*` request. **Until you set one, the API is disabled** (the page loads but shows nothing).

Open `http://127.0.0.1:4269/`, enter the token, and you get summary cards, Accounts (Manage / Launch / Close), Characters, an Empire timeline, and a **Clients** tab with live low-res MJPEG video of each running client.

> **Security:** every `/api/*` call needs the token (constant-time compared), cross-origin requests are rejected, security headers are set, and concurrent connections are capped. Traffic is **plain HTTP** — for remote access put it behind a TLS reverse proxy and/or a firewall allow-list and use a long random token. Forwarding the port to the open internet without those is unsafe.

---

## Notifications, logging & updates

- **Logging** — major issues append to `LOG_FILE` (default `xa-sub.log`) when `ENABLE_LOGGING` is on; `DEBUG: true` mirrors every line. Crashes write `crash_log_<ts>.log` + a `.dmp` minidump.
- **Discord** — `ENABLE_DISCORD_WEBHOOK` + `DISCORD_WEBHOOK_URL` (HTTPS Discord host).
- **Pushover** — `ENABLE_PUSHOVER` + `PUSHOVER_USER_KEY` + `PUSHOVER_API_TOKEN`.
- **Automatic game updates** — `FORCE_UPDATE_ALL_GAMES` (off by default) lets the app handle FFXIV patches: it closes every client, confirms XIVLauncher's prompt, waits up to `GAME_UPDATE_TIMEOUT` (default 1 hour), then relaunches and re-checks each managed client. Enabling it asks for confirmation because it can close all running clients.

---

## Data, safety & privacy

- **Reads your plugin data, drives your clients.** XA Sub only **reads** AutoRetainer / Lifestream / `xa.db` files your plugins already wrote — it never modifies plugin data. It does **launch, arrange, and close** game clients (that's the point of automation), so leave automation **off** until you've reviewed your setup.
- **Local-only by default.** The web server binds to `127.0.0.1` and is off unless you enable it. Nothing is uploaded. The only outbound calls are the optional Discord/Pushover notifications, which **you** configure.
- **Secrets on disk.** When XA Sub saves `config.json`, it locks the file to an owner-only ACL. Prefer `keyring_name` (Windows Credential Manager) over an inline `otp_secret`.
- **No telemetry, no account, no login.**
- Use the in-app **Privacy toggle** before sharing screenshots.

---

## FAQ

**Is there a native Linux build?**
No — it's a Windows app that runs under Wine on Linux. See [Linux (via Wine)](#linux-via-wine).

**Do I have to use the installer?**
No — a portable zip is also provided: extract and run `XA Sub.exe`. Either way there is no .NET/runtime to install. The installer just adds a Start Menu entry and an uninstaller.

**Do I have to edit `config.json` before the first run?**
No — with a single default XIVLauncher install your data shows up immediately. Edit it (or use Import) only to add more accounts or change defaults.

**Where's the version?**
In the title bar and **Settings → General → Version** (v1.00).

**Why does SmartScreen warn me?**
The exe is unsigned. Choose **More info → Run anyway**.

**Can I change the web port?**
Yes — Settings → Web Server, or set `WEB_SERVER_PORT`.

**Does it modify my game or get me banned?**
It doesn't modify plugin data, but it **does** automate the game client (launch/login/close). Automating FFXIV can violate the User Agreement and risk your account — see the Disclaimer.

**Is the source code available?**
No. XA Sub Manager is proprietary, closed-source software distributed as compiled binaries. See [LICENSE](LICENSE).

---

## Support

Found a bug or have a request? Reach us through the official page at [aethertek.io](https://aethertek.io/applications/xa-sub-manager.html) or the community Discord linked there.

---

## Disclaimer

Use of third-party tools and automation may violate the FINAL FANTASY XIV User Agreement and can put your account at risk, including suspension or termination. This software is provided "as is", for personal use, with no warranty. You are solely responsible for how you use it.

## License

XA Sub Manager is **proprietary software**. The compiled binaries are free to download and use for personal, non-commercial play; redistribution, resale, modification, and reverse-engineering are **not** permitted. See the full [End User License Agreement](LICENSE) and [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md) for bundled third-party components.

> FINAL FANTASY and FINAL FANTASY XIV are registered trademarks of SQUARE ENIX Holdings Co., Ltd. This project is unofficial and not affiliated with or endorsed by SQUARE ENIX.

© 2026 XA / xa-io. All Rights Reserved.
