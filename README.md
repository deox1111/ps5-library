<p align="center"><img src="docs/icon.png" width="128" alt="PS5 Library"></p>
<h1 align="center">PS5 Library</h1>
<p align="center"><b>A game library for jailbroken PS5.</b><br>Pick a game, verify its download on your PS5, play it from your home screen.</p>

<p align="center">
  <a href="../../releases/latest"><img src="https://img.shields.io/github/v/release/deox1111/ps5-library?label=release&color=3d7bff" alt="Latest release"></a>
  <a href="../../releases"><img src="https://img.shields.io/github/downloads/deox1111/ps5-library/total?color=3d7bff" alt="Downloads"></a>
  <img src="https://img.shields.io/badge/language-C%2B%2B17-00599C?logo=cplusplus&logoColor=white" alt="Language: C++17">
  <img src="https://img.shields.io/badge/platform-PS5-003791?logo=playstation&logoColor=white" alt="Platform: PS5">
  <img src="https://img.shields.io/badge/tested%20on-FW%2013.60-555" alt="Tested on firmware 13.60">
  <a href="LICENSE"><img src="https://img.shields.io/badge/license-GPL--3.0--or--later-blue" alt="License: GPL-3.0-or-later"></a>
  <a href="#donate"><img src="https://img.shields.io/badge/donate-XMR-FF6600?logo=monero&logoColor=white" alt="Donate XMR"></a>
</p>

<p align="center"><img src="docs/store.jpg" alt="Store"></p>

## Features

- **One payload.** `ps5-library.elf` adds a **PS5 Library** tile to your home screen. The tile opens the library in the PS5 browser.
- **Feels like the console.** The game you select fills the screen, rows of games sit underneath, and every game has a full-screen page. The layout gets denser in the PS5 browser's window, so more of the library fits.
- **Install from the console.** After you complete the host's verification, the console downloads, unpacks and installs the game. When it's done, the game is on your home screen.
- **Internal SSD or USB.** Pick a drive per game, or set a default. Games in exFAT/FFPKG format run from that drive through ShadowMountPlus. PKG games go through the system installer.
- **Downloads resume** after a pause, a dropped connection, a failed attempt or a console restart. Partial files are cleaned up after the install.
- **Room for the queue.** The game page shows the space free on the drive, what unfinished downloads still need and what is left after them and this game. A download that would not fit asks first.
- **Queue in your order.** **Downloads** has Active, Finished, Failed and Cancelled tabs. A waiting download can move up, move down or go next, and finished, failed or cancelled downloads clear in one go.
- **Favourites.** The heart on a game page adds it to your favourites: they get their own row in the Store and a filter in **Browse**. The list is kept on the console, so the console and your phone share it.
- **Parallel downloads.** Up to 4 connections per file by default; choose 1, 4 or 8 in Settings. With 8 connections a wired console reached a full 1 Gbps line. Hosts without compatible range support use one connection.
- **Made for the controller.** Move with the D-pad, select with Cross, go back with Circle.
- **Files.** A simple file manager in the top bar for the internal SSD and USB drives: unpack archives, install what you copied over, copy or move between drives, rename, delete. See [Files](#files).
- **Several catalogs at once.** Every catalog gets its own row in the Store, games first and homebrew after them. Catalogs made for Pegasus DL work too. A homebrew catalog is on from the start.
- **Covers and details:** size, region and minimum firmware for every game.
- **Browse by region and size.** Filter by region and size, and sort by size. The region comes from the catalog, or from the tags in its description.
- **Catalog versions and backports.** Game versions, reported backport revisions/firmware targets, and the original download-source labels stay visible. Dump, FPKG, DLC and Backport links can be distinguished before downloading.
- **Phone and computer.** Open `http://<ps5-ip>:9999` on your phone or PC to queue games from the couch (19999 if [another payload uses 9999](#troubleshooting)). Phones get a one-column layout with a tab bar at the bottom. Each device pairs once with a code from the console: see [Phones and computers](#phones-and-computers).
- **Updates from the library.** **Settings → Updates** finds a new release, checks it and switches to it. See [Update](#update).
- **Diagnostic report.** **Settings → Console → Diagnostic report** collects what a bug report needs, without keys, passwords, catalog addresses or download links.
- **Browser downloads.** Open a host on your PS5, complete its verification and press Download. The library captures a usable file link and adds it to Downloads. A phone or computer can also supply a download link.
- **Link-Vault hosts.** The PS5 reads the file list, opens the selected host automatically and remembers its links and filenames, including multipart downloads.
- **Optional accounts.** An AllDebrid or TorBox account, or a host's premium key, downloads from supported hosts directly: the game page offers **Download with AllDebrid** (or the account you saved) next to the browser verification.

## Download speed

In **Settings → Download speed**, choose **1, 4 or 8 connections** per file. The default is **4**. Changes apply to the next download or resume; pause and resume an active download to apply them.

Large files can download in parallel when the host provides byte ranges and a stable file identifier (strong ETag). Free connections pick up the next part without waiting for a whole batch. Interrupted parts retry individually; temporary host rate limits reduce concurrency, then the library gradually tries to increase it again after a quiet period. Unsupported or repeatedly refused ranges still fall back safely to one connection.

Downloads shows active versus selected connections (for example **4 / 8**) and explains retries or fallback. The speed readout uses a short moving average. The library still checks every part, preserves pause/resume and bounds its memory use; fewer connections near the end of a file or while waiting for retries are normal.

Since 0.7.21, each connection asks the console for a 512 KiB TCP receive buffer (the console's default of 64 KiB limited every connection to roughly 1–6 MB/s), finished parts are written on their own thread, an idle connection takes over half of a part that holds the others up, and a connection that stops delivering for 3 seconds is replaced. A line under a running download shows each connection's speed, storage load, read-ahead use and these takeovers.

Parallel connections can help when a host limits each connection separately. They cannot increase your connection's capacity or remove a host's account-wide speed limit. Try 4 first; choose 8 if it helps with your host, or 1 for compatibility.

On Wi-Fi, the console's wireless link is often the limit. A cable works best; otherwise use the 5 GHz band (**Settings → Network → Settings → Set Up Internet Connection**, press **Options** on your network, **Wi-Fi Frequency Bands → 5 GHz**).

## Files

**Files** in the top bar browses the internal SSD (`/data`) and your USB drives. Move with the D-pad: the selected item's details and actions appear on the right (on a phone, right below the item you tap).

- **Unpack here** extracts a ZIP, 7z or RAR archive, split sets too (`.7z.001`, `.part1.rar`), into a new folder next to it. The archive stays.
- **Install** a game image (`.exfat`, `.ffpkg`, `.ffpfs`, `.ffpfsc`), a package, an archive or a game folder you copied over. It goes through the same steps as a download.
- **Copy** or **Move**, open the target folder, then **Paste here**. On the same drive a move is instant; between drives it runs in Downloads with progress, and can be paused or cancelled. The copy only appears once it is complete, and a move deletes the original last.
- **Rename**, **New folder**, **Delete**, a filter and sorting by name, size or date. On a phone or computer, **Download** saves a file to that device.
- Drive roots, the library's own settings files and the folders of a running job are protected.

**Show in Files** in My games, **Show files** in Downloads and **Browse** in Settings → Downloads open the matching folder.

## Phones and computers

Open `http://<ps5-ip>:9999` in a browser on the same network; **Settings → Devices** on the console shows the address. A phone or computer pairs once before it can use the library. The console's own browser never needs to.

- **Scan the QR code** in **Settings → Devices** on the console. It opens the library and pairs the phone in one step.
- Or open the address and choose **Show code on PS5**. The console shows a six-digit code in a notification; enter it and choose **Pair**.

A code works once and for five minutes; five wrong codes cancel it. The browser then stays paired. **Settings → Devices** lists every paired phone and computer, and **Remove** locks one out again. The console keeps only a hash of each device's key. To let every device on your network in without pairing, set `"pairing": false` in `/data/ps5-library/config.json` and load the library again.

On a phone the library uses one column, large touch targets and a tab bar along the bottom.

<p align="center">
  <img src="docs/phone-store.jpg" width="250" alt="Store on a phone">
  <img src="docs/phone-game.jpg" width="250" alt="Game page on a phone, with the free space before downloading">
  <img src="docs/phone-downloads.jpg" width="250" alt="Downloads on a phone">
</p>

## Downloads that need a browser

Choose **Solve CAPTCHA → Open on PS5**, complete the host's check yourself, then press its **Download** button. Every catalog host uses this path, including cached Link-Vault mirrors and hosts that sometimes work without a CAPTCHA. The Store, game page and individual versions no longer guess availability from the host's name. If the selected URL already serves a real file, the library verifies it and queues it directly. For multipart downloads, it opens the next part in turn and returns to Downloads after collecting the links. Files from GitHub releases show **Download** instead: there is no CAPTCHA, and the console checks the file and adds it to Downloads.

If you leave the host or get redirected, return to the library and press **Retry** next to **Cancel**. Retry reopens the current page, renews its waiting time and keeps earlier captured parts while the session is active. It also rechecks links that failed verification during that part. After a failed session, Retry starts a fresh attempt with the same game, mirror and drive. Queued downloads are managed in **Downloads**.

If verification cannot run in the console browser, press **Change host or device**. The library stops capture before returning to the mirror selector and **Use phone or computer**. A new attempt starts from the first part.

**WebAssembly verification:** a clean local test on PS5 firmware 13.60 returned `typeof WebAssembly === "undefined"`. The library launches the system browser with its default options and does not disable WebAssembly. A host such as FileDitch that requires it may remain blocked even with ad blocking off. This update provides an alternative path; it does not enable WebAssembly or fix that host's native verification. Use another mirror, or try the existing phone/computer flow on the same network (links bound to browser cookies may still fail).

![Open a download host on PS5](docs/browser.jpg)

If a site does not work in the PS5 browser, choose **Use phone or computer**. Use the same Wi-Fi as the console, complete verification there, and copy the file's download link into the library.

<p align="center"><img src="docs/browser-phone.jpg" width="300" alt="Phone or computer fallback"></p>

For **Link-Vault**, the first attempt briefly opens Link-Vault on the PS5. Complete its verification if asked; the library reads the file list and opens your selected host automatically. Links and filenames are saved on the console, so later attempts go straight to the host. No PC, VPS or Telegram connection is needed.

Browser capture is tested on firmware 13.60 with controlled multipart files, and Link-Vault resolution is tested on the console through to the selected hosting page. Compatibility varies by host; links requiring browser cookies or a different browser may not work. CAPTCHA verification is manual.

### DataNodes popup ads

The DNS workaround below is experimental; reliable blocking in the PS5 browser has not been verified. Adblock work is currently deferred.

DataNodes has two download steps; the first button does not provide the file yet. Some clicks can trigger a third-party popup script. If your PS5 already uses [nanoDNS](https://github.com/drakmor/nanoDNS), add these two rules inside the existing `[overrides]` section of `/data/nanodns/nanodns.ini`, then reload nanoDNS and reopen the hosting page:

```ini
dcbbwymp1bhlf.cloudfront.net=127.0.0.1
d3jzhqnvnvdy34.cloudfront.net=127.0.0.1
```

The console must actually use nanoDNS: in the PS5 connection's manual DNS settings, use nanoDNS's bind address as the primary DNS (normally `127.0.0.1`) and leave the secondary unset (`0.0.0.0`), so another resolver cannot bypass the rules. Fully close the browser before trying again. Keep nanoDNS running while using this DNS setup.

These are the two popup script domains observed on October 7, 2026. Keep the remaining nanoDNS rules; do not block all of `cloudfront.net`. The host's countdown and CAPTCHA still need to be completed. The Library ELF does not install or configure nanoDNS automatically. Ad providers can change their domains, so this is a targeted block, not a universal popup blocker.

## Requirements

- A jailbroken PS5 with an ELF loader. Tested on firmware 13.60.
- Free space: an archive (7z, rar, zip) and its **full unpacked contents** must fit together during installation. Highly compressed archives can expand to much more than twice the download size. The game page shows the space free now, what unfinished downloads on that drive still need and what is left after them and this game; a download that would not fit asks for confirmation. The library checks again before downloading and for each file during extraction. The downloaded archive is deleted after a successful install.
- [kstuff](https://github.com/EchoStretch/kstuff-lite)
- [ShadowMountPlus](https://github.com/drakmor/shadowMountPlus). With 1.7beta4 or newer, **My games** also lists games installed from PKG files, PS4 games included.

## Install

1. Download `ps5-library.elf` from [Releases](../../releases/latest).
2. Load it with your ELF loader (port 9021) or your payload manager. Add it to **autoload**: the home screen tile only works while the payload is running.
3. A notification says **PS5 Library added to the home screen**. Open the tile.
4. The homebrew catalog [evoX-CoreOS](#the-default-catalog) is already on. Add more in **Settings → Catalogs**, see [Catalogs](#catalogs) below.

### Update

**Settings → Updates** (0.7.22 or newer) shows the running version and the latest release on GitHub, with what is new. The library checks shortly after it starts and twice a day, and shows a notification once per new version. Nothing installs on its own; automatic checks can be turned off there.

**Install** downloads `ps5-library.elf` and checks it against the release's SHA-256 file. A file that does not match replaces nothing. Then it writes the new version over every copy of PS5 Library that your console starts: the file your payload manager or autoloader loads at boot (Payload Manager, etaHEN or another payload folder, the root of a USB drive) and the library's own copy in `/data/ps5-library`. That way the next start loads the new version too. A file only counts as a copy when it is named `ps5-library….elf` and contains this library; other payloads are never touched. The new version then starts and takes over. Catalogs, settings, accounts, paired devices and downloads stay; a download in progress pauses and **Resume** continues it. While a package is installing, wait until it finishes.

Payload Manager can list PS5 Library too: in its **Settings → Manage Sources → Add Source**, add `https://raw.githubusercontent.com/deox1111/ps5-library/main/payloads.json`.

To update by hand, load the new ELF. Version 0.7.8 and newer replace whatever version is running (a notification says so) and keep your catalogs, settings, accounts and downloads. A download in progress pauses; **Resume** continues it.

To reset, delete `/data/ps5-library` and load the ELF again (0.7.8 or newer); the running library stops and starts fresh. An update never resets anything.

## Installation guide: firmware, formats and backports

The guide opens expanded on the game page, in download-source selection and in **Settings → Installation guide**. **Settings → Console** also shows the detected console firmware.

On detected PS5 firmware above 11.60, source selection marks PS5 game sources that the catalog labels as FPKG (fake game packages) as unsupported and asks you to choose a Dump or image instead. A source's own Dump/image label takes precedence over the format of a mixed catalog entry. PS4 titles and packages in a catalog's PKG category, such as the apps in the default homebrew catalog, are not affected. Unknown firmware or source formats are explained rather than guessed; this format check does not verify that every game or backport will run.

- **PS5 firmware above 11.60:** choose a compatible extracted game folder (**Dump**) or a ShadowMountPlus image: `.exfat`, `.ffpkg`, `.ffpfs` or `.ffpfsc`. PS5 game FPKG support is documented through **11.60 inclusive** in [kstuff-lite v1.11](https://github.com/EchoStretch/kstuff-lite/releases/tag/v1.11); support for the payload on newer firmware does not imply support for PS5 fake `.pkg` games there. Guidance checked October 8, 2026.
- **FPKG and FFPKG are different.** A fake PKG is a `.pkg` installed through the package installer. A `.ffpkg` is a UFS image mounted by ShadowMountPlus. See [ShadowMountPlus's format table](https://github.com/drakmor/ShadowMountPlus/blob/1.7/README.md#current-image-support). The PS5 game FPKG warning is not a blanket restriction on PS4 packages or homebrew.
- **Choose the right source.** A catalog can put Dump, FPKG, DLC and Backport links under the same game. Read the source label: DLC or backport downloads may contain only an add-on or patch. Install the matching base game first and follow the author's instructions.
- **Version and backport are separate.** The library shows the game version and, when supplied, the backport revision and target firmware. A listed `4.xx` target is the catalog author's claim, not a verified minimum for every source. A backport may be a separate download; missing metadata is left unspecified.
- **Installation:** choose a source and drive, complete host verification, and let the library download/extract supported archives. Folders and images are registered with ShadowMountPlus; keep those files on their drive. Compatible `.pkg` files go to the system installer—check the console's installation result.

**Waiting for ShadowMount:** registration is confirmed against the installed source path. Pending registrations stay in **Needs attention**, with the Install step pending; Ready is shown only after confirmation. The library rechecks while running and viewing the queue, including jobs left pending by an earlier version. Use **Retry scan** to ask ShadowMount to rescan existing files without downloading or moving them again. If the scan is deferred because a game is running, close the game and return to the console home screen. Finished entries remain as history until you clear them; removing an installed entry from this list does not uninstall its game.

## Catalogs

PS5 Library does not publicly include, host or link to any games. The Store shows the games from the catalogs you add. A catalog is a JSON file at a web address.

### Add a catalog

> **Tip:** typing a long address with the controller is slow. Open `http://<ps5-ip>:9999` on your phone or PC and paste the address there.

1. Open **Settings → Catalogs**.
2. **Add a catalog:** the full address of the JSON file. It must start with `http://` or `https://`.
3. **Access key:** fill it in only if the catalog's owner gave you a key. Otherwise leave it empty. The key is sent as `Authorization: Bearer <key>`.
4. Press **Add catalog**. The library reads the catalog straight away and its games show up in the Store.

Good to know:
- You can add up to 30 catalogs. The Store shows the games of every catalog that is on. When two catalogs have the same game, its page lists both.
- **Turn off** hides a catalog's games, **Remove** deletes the catalog from the list.
- The library checks catalogs by itself: catalogs with a key every minute, the others every 10 minutes. **Refresh all** checks them now. If a catalog goes offline, you keep its last copy.
- **Browse** can show the games of one catalog only.

### Catalogs on GitHub

Use the **Raw** address. Open the file on GitHub, press **Raw** and copy the address from the browser. The normal file page is a web page, not the JSON file.

| ✗ Doesn't work | ✓ Works |
| --- | --- |
| `https://github.com/user/repo/blob/main/catalog.json` | `https://raw.githubusercontent.com/user/repo/main/catalog.json` |

### Pegasus DL catalogs

Catalogs made for [Pegasus DL](https://github.com/pegasus-ps5/pegasus-dl) work as they are: add the catalog's address like any other. Both Pegasus package shapes are read (`downloadLinks`, or `url` with `filename`), and every link of a package becomes a download mirror.

A Pegasus **source list** (a file with a `"sources": [...]` list) has no games in it, it names other catalogs. Adding a source list adds every catalog it names, turned on or off as the list says.

### The default catalog

[evoX-CoreOS](https://github.com/nexgen999/evoX-CoreOS) by nexgen999 is on from the start: homebrew apps and community utilities. Turn it off or remove it in **Settings → Catalogs**; a removed catalog does not come back.

### Make your own catalog

The smallest catalog has one entry with one file:

```json
{
  "entries": [
    {
      "title": "My Homebrew App",
      "files": [{"url": "https://example.com/my-app.pkg"}]
    }
  ]
}
```

Put the file anywhere that serves files over HTTP(S):
- **GitHub:** add `catalog.json` to a repository or a gist, then connect its Raw address.
- **Your PC:** in the folder with `catalog.json`, run `python -m http.server 8000` and connect `http://<pc-ip>:8000/catalog.json`.

A complete entry, with a second mirror:

```json
{
  "entries": [
    {
      "id": "my-game-exfat",
      "title": "Game name (v1.02)",
      "title_id": "PPSA01234",
      "kind": "game",
      "platform": "ps5",
      "format": "exfat",
      "size": "52GB",
      "region": "EUR",
      "firmware": "4.xx",
      "files": [{"url": "https://mirror-one.example/PPSA01234.7z", "name": "PPSA01234.7z"}],
      "source_sets": [
        {"host": "mirror-one.example", "files": [{"url": "https://mirror-one.example/PPSA01234.7z", "name": "PPSA01234.7z"}]},
        {"host": "mirror-two.example", "files": [{"url": "https://mirror-two.example/PPSA01234.7z", "name": "PPSA01234.7z"}]}
      ]
    }
  ]
}
```

| Field | | What it does |
| --- | --- | --- |
| `entries` | **required** | The list of games. It must contain at least one entry. |
| `files` | **required** | The files to download: `{"url": "...", "name": "..."}`. `name` is optional; without it the name comes from the URL. Several files make one download, for example the parts of a split archive (`.7z.001`, `.7z.002`). |
| `title` | recommended | The name in the Store. Without it the first file name is used. A version in brackets, like `(v1.02)`, is shown as the version. |
| `title_id` | recommended | `PPSA01234` or `CUSA01234`. Brings the cover and background art, groups a game with its DLC and updates, marks it **Ready to play** once installed, and checks that the download is the right game. |
| `kind` | | `game` (default), `dlc` or `update`. |
| `platform` | | `ps5` (default) or `ps4`. Used for the badge and the platform filter. |
| `format` | | `exfat`, `ffpkg`, `ffpfs`, `ffpfsc`, `fpkg` or `pkg`. Shown on the game page. When a game is listed in several formats, exFAT is offered first. The installer finds the real type by itself. |
| `size` | | Download size, like `"52GB"` or `"700 MB"`. Used for **Quick downloads** and for the free space warning. |
| `region`, `firmware`, `version` | | Shown on the game page. `firmware` is the minimum firmware, like `"4.xx"`. |
| `description` | | A few lines shown on the game page. |
| `cover` | | Address of a cover image, used when the PlayStation Store has none (homebrew). |
| `password` | | The archive's password. |
| `source_sets` | | Alternative mirrors: `[{"host": "...", "files": [...]}]`. Put the first mirror's files in `files` too. Choose a mirror in **Solve CAPTCHA**; `host` labels it, and does not mark it as CAPTCHA-free. API downloads can still try mirrors in order. |
| `id` | | Your own ID for the entry, unique within the catalog. |

What a download can be:
- A `.pkg`, an exFAT/FFPKG/FFPFS image, or an archive with one of them inside (`.7z`, `.zip`, `.rar`, `.tar`).
- Direct download links and host pages use **Solve CAPTCHA → Open on PS5**; files from GitHub releases use **Download**. Direct files are checked automatically; pages open for verification.
- **AllDebrid:** paste your API key in **Settings → Download accounts**. The library checks it, shows your plan and saves the hosts AllDebrid supports for your account. Sources on those hosts get **Download with AllDebrid**: the link is unlocked through the AllDebrid API (delayed links included) and downloads and installs like any other. Expired links are unlocked again; dead links, unsupported hosts, quota and API errors fall back to the normal flow and show AllDebrid's reason in Downloads. The key stays on the console.

## Screenshots

| Game page | Browse |
| --- | --- |
| ![Game page](docs/game.jpg) | ![Browse](docs/browse.jpg) |
| **Downloads** | **Settings → Catalogs** |
| ![Downloads](docs/downloads.jpg) | ![Settings → Catalogs](docs/catalogs.jpg) |
| **Settings → Devices** | **Settings → Updates** |
| ![Settings → Devices](docs/devices.jpg) | ![Settings → Updates](docs/updates.jpg) |

## Troubleshooting

- **Reporting a problem.** Attach the report from **Settings → Console → Diagnostic report** (0.7.24 or newer): **Copy** it on a phone or computer, or save it on the console as `diagnostics.txt` in the library's folder. Read it before you share it; download links in it are cut down to their host.
- **The tile cannot connect after Rest Mode.** Update to 0.7.18. A failed HTTP listener now reopens automatically on the same port; recovery was verified on firmware 13.60 without relaunching the payload. Since 0.7.21 a notification says when the library is active again. If the console or loader terminates the whole process, the payload still needs to be loaded again.
- **A phone or computer shows "Pair this device".** Since 0.7.22 every phone and computer pairs once. Scan the QR code in **Settings → Devices** on the console, or choose **Show code on PS5** and enter the code from the notification. A private browser window forgets the pairing when it closes.
- **An update was saved but did not start.** No ELF loader or Payload Manager answered on the console. The saved copies already hold the new version: load `ps5-library.elf` from your payload manager.
- **Downloads are much slower than on a computer.** Update to 0.7.21 and use 4 or 8 connections. The line under the download shows the speed of each connection. If the total stays near your Wi-Fi speed, the console's wireless link is the limit: see [Download speed](#download-speed).
- **The tile opens an empty page on startup.** Make sure the payload is running. Load it, or add it to autoload.
- **Can the library go full screen?** No. The PS5 browser has no full-screen mode for web pages; its bars stay on screen. The library uses a denser layout in that window instead.
- **A "?" appears instead of a symbol.** Update to 0.7.20. The console's fonts lack some symbols (such as the Cross button sign); they are drawn as icons now.
- **Covers are black in the PS5 browser.** Older firmware browsers (for example 5.10) could not place the cover images before 0.7.8. Update.
- **"PS5 Library is already running".** The same version already runs, so the new launch exits. A different version replaces it (0.7.8 or newer).
- **Another payload uses port 9999.** PS5 Library moves to port **19999** (then 29999) and says so in a notification. It stays on that port on later launches, and the home screen tile follows it. On your phone or PC, open `http://<ps5-ip>:19999`. To pick the port yourself, set `"port"` in `/data/ps5-library/config.json`.
- **"No catalog answered at that address".** Open the address in a browser on your PC. If the browser can't open it either, the catalog is offline or the address is wrong. If the catalog has a key, check the key.
- **"Not enough free space" while unpacking.** The message identifies the current file and its required/available space, not the total for the whole archive. Free space, then choose **Try again**; unpacking restarts from the saved archive. If even the error cannot be saved on a full disk, it stays visible in the running library but may not survive a payload restart.
- **Unpacking appears stuck.** Version 0.7.18 shows the current file, written bytes and a notice after a longer period without new output. An unknown archive total is not shown as a percentage. A pause or error remains on the **Unpack** step; retrying extraction starts it again rather than resuming inside the compressed stream. If unpacking finished and a later step failed, **Try again** continues from the unpacked files (0.7.23 or newer).
- **Unpacking is slow.** From 0.7.23 the download card shows how fast the game is written and the archive read, files per second, and whether the decoder (the console's CPU) or storage sets the pace. Strongly compressed archives are limited by the CPU; games made of many small files are limited by how fast the console's storage creates files.
- **"Directory scan limit exceeded" after unpacking.** Versions before 0.7.23 listed every file of the unpacked game and stopped at 100,000 files. Update, then choose **Try again**; a game unpacked by an older version is unpacked once more.
- **"That address returns a web page, not a catalog".** Use the direct file address. On GitHub, use the [Raw](#catalogs-on-github) one.
- **"ShadowMountPlus is not running".** Start ShadowMountPlus, for example through your autoloader.
- **ShadowMountPlus reports failed installs, or a game stays at "Waiting for ShadowMount registration".** After a game launch or Rest Mode, ShadowMountPlus can lose its install hook ([drakmor/ShadowMountPlus#145](https://github.com/drakmor/ShadowMountPlus/issues/145), [#147](https://github.com/drakmor/ShadowMountPlus/issues/147)); its log then says "internal AppInstallTitleDir bridge unavailable" and no new game registers until it restarts. Choose **Restart ShadowMount** on that download (0.7.21); close any game that runs from an image or game folder first. From 0.7.25 the download shows this within seconds of ShadowMountPlus's attempt. If the library can't find the ShadowMountPlus file, set `"shadowmount_elf"` in `/data/ps5-library/config.json` or restart ShadowMountPlus from your payload manager.
- **PS4 games or games installed from PKG files are missing in My games.** Update ShadowMountPlus to 1.7beta4 or newer; older versions only list the games they mount.
- **A game shows "CAPTCHA / browser".** Choose **Solve CAPTCHA → Open on PS5**. This is available for every host. Files from GitHub releases show **Direct download** and a **Download** button instead. A saved account (AllDebrid, TorBox or a host's premium key) that supports the host also offers **Download with** that account.
- **A captured link fails with "filename exceeds 200 bytes".** Update to 0.7.11. Long signed URLs could incorrectly be interpreted as filenames even when the browser supplied the correct name.
- **A download fails with "returned a web page instead of a file" or "needs a browser CAPTCHA".** The host only gives the file to a browser. Choose **Solve CAPTCHA** on that download. If the captured link doesn't work, pick another mirror.

## Credits

Made by **deox**.

Shout-outs:
- **Pippo26442999**
- **[ps5upload](https://github.com/phantomptr/ps5upload)** by phantomptr. PS5 Library's package installer is built on it.
- **[Pegasus DL](https://github.com/pegasus-ps5/pegasus-dl)**. Its catalogs work in PS5 Library too.
- [evoX-CoreOS](https://github.com/nexgen999/evoX-CoreOS) by nexgen999, the default homebrew catalog
- [ShadowMountPlus](https://github.com/drakmor/shadowMountPlus) by drakmor
- [kstuff](https://github.com/EchoStretch/kstuff-lite)
- [ps5-payload-sdk](https://github.com/ps5-payload-dev/sdk) by John Törnblom
- [cpp-httplib](https://github.com/yhirose/cpp-httplib), [nlohmann/json](https://github.com/nlohmann/json), curl, libarchive, OpenSSL

## Donate

If PS5 Library saves you time, you can support it with Monero (XMR):

```
4A66wqfp2Lp1TyBC5DGYA1DNYUv5RB1zPASbmCaus78PCbcEfvp3cjbR2HxFmnYbEVgZ7NZCCsuBcYLLnUuZYjej54YLoFS
```

<img src="docs/xmr.svg" width="160" alt="Monero address QR code">

## Disclaimer

PS5 Library is not affiliated with Sony Interactive Entertainment. PS5 Library is a downloader and local file-management tool. It does not include package catalogs, provide package links, bypass accounts, spoof PSN, bypass anti-cheat, or unlock content.
Use it only with content you own or have permission to download.

## License

GPL-3.0-or-later, see [LICENSE](LICENSE). The source code will be published in this repository.
