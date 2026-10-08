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
- **Feels like the console.** The game you select fills the screen, rows of games sit underneath, and every game has a full-screen page.
- **Install from the console.** After you complete the host's verification, the console downloads, unpacks and installs the game. When it's done, the game is on your home screen.
- **Internal SSD or USB.** Pick a drive per game, or set a default. Games in exFAT/FFPKG format run from that drive through ShadowMountPlus. PKG games go through the system installer.
- **Downloads resume** after a pause, a dropped connection, a failed attempt or a console restart. Partial files are cleaned up after the install.
- **Parallel downloads.** Up to 4 connections per file by default; choose 1, 4 or 8 in Settings. Hosts without compatible range support use one connection.
- **Made for the controller.** Move with the D-pad, select with Cross, go back with Circle.
- **Several catalogs at once.** Every catalog gets its own row in the Store, games first and homebrew after them. Catalogs made for Pegasus DL work too. A homebrew catalog is on from the start.
- **Covers and details:** size, region and minimum firmware for every game.
- **Any browser.** Open `http://<ps5-ip>:9999` on your phone or PC to queue games from the couch (19999 if [another payload uses 9999](#troubleshooting)).
- **Browser downloads.** Open a host on your PS5, complete its verification and press Download. The library captures a usable file link and adds it to Downloads. A phone or computer can also supply a download link.
- **Link-Vault hosts.** The PS5 reads the file list, opens the selected host automatically and remembers its links and filenames, including multipart downloads.
- **Optional accounts.** A premium key or a TorBox account can download from supported hosts directly.

## Download speed

In **Settings → Download speed**, choose **1, 4 or 8 connections** per file. The default is **4**. Changes apply to the next download or resume; pause and resume an active download to apply them.

Large files can download in parallel when the host provides byte ranges and a stable file identifier (strong ETag). The library checks every part before writing it, preserves pause/resume, and falls back to one connection if the host rejects parallel requests. Downloads shows the number of connections currently in use.

Parallel connections can help when a host limits each connection separately. They cannot increase your connection's capacity or remove a host's account-wide speed limit. Try 4 first; choose 8 if it helps with your host, or 1 for compatibility.

## Downloads that need a browser

Choose **Solve CAPTCHA → Open on PS5**, complete the host's check yourself, then press its **Download** button. Every catalog host uses this path, including cached Link-Vault mirrors and hosts that sometimes work without a CAPTCHA. The Store, game page and individual versions no longer guess availability from the host's name. If the selected URL already serves a real file, the library verifies it and queues it directly. For multipart downloads, it opens the next part in turn and returns to Downloads after collecting the links.

If you leave the host or get redirected, return to the library and press **Retry** next to **Cancel**. Retry reopens the current page, renews its waiting time and keeps earlier captured parts while the session is active. It also rechecks links that failed verification during that part. After a failed session, Retry starts a fresh attempt with the same game, mirror and drive. Queued downloads are managed in **Downloads**.

![Open a download host on PS5](docs/browser.jpg)

If a site does not work in the PS5 browser, choose **Use phone or computer**. Use the same Wi-Fi as the console, complete verification there, and copy the file's download link into the library.

![Phone or computer fallback](docs/browser-phone.jpg)

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
- Free space: a game packed as an archive (7z, rar, zip) needs about **twice its size** free while it installs, because the archive and the unpacked game are on the drive together. The archive is deleted after the install. The library checks the space before it downloads.
- [kstuff](https://github.com/EchoStretch/kstuff-lite)
- [ShadowMountPlus](https://github.com/drakmor/shadowMountPlus). With 1.7beta4 or newer, **My games** also lists games installed from PKG files, PS4 games included.

## Install

1. Download `ps5-library.elf` from [Releases](../../releases/latest).
2. Load it with your ELF loader (port 9021) or your payload manager. Add it to **autoload**: the home screen tile only works while the payload is running.
3. A notification says **PS5 Library added to the home screen**. Open the tile.
4. The homebrew catalog [evoX-CoreOS](#the-default-catalog) is already on. Add more in **Settings → Catalogs**, see [Catalogs](#catalogs) below.

To update, load the new ELF. Version 0.7.8 and newer replace whatever version is running (a notification says so) and keep your catalogs, settings, accounts and downloads. A download in progress pauses; **Resume** continues it.

To reset, delete `/data/ps5-library` and load the ELF again (0.7.8 or newer); the running library stops and starts fresh. An update never resets anything.

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
- Direct download links and host pages use **Solve CAPTCHA → Open on PS5**. Direct files are checked automatically; pages open for verification. Download accounts remain available for API and existing queue downloads.

## Screenshots

| Game page | Browse |
| --- | --- |
| ![Game page](docs/game.jpg) | ![Browse](docs/browse.jpg) |
| **Downloads** | **Settings → Catalogs** |
| ![Downloads](docs/downloads.jpg) | ![Settings → Catalogs](docs/catalogs.jpg) |

## Troubleshooting

- **The tile opens an empty page.** The payload isn't running. Load it again, or add it to autoload.
- **Covers are black in the PS5 browser.** Older firmware browsers (for example 5.10) could not place the cover images before 0.7.8. Update.
- **"PS5 Library is already running".** The same version already runs, so the new launch exits. A different version replaces it (0.7.8 or newer).
- **Another payload uses port 9999.** PS5 Library moves to port **19999** (then 29999) and says so in a notification. It stays on that port on later launches, and the home screen tile follows it. On your phone or PC, open `http://<ps5-ip>:19999`. To pick the port yourself, set `"port"` in `/data/ps5-library/config.json`.
- **"No catalog answered at that address".** Open the address in a browser on your PC. If the browser can't open it either, the catalog is offline or the address is wrong. If the catalog has a key, check the key.
- **"Not enough free space".** The message says how much the game needs. Free up space, or pick another drive on the game page (**Change drive**).
- **"That address returns a web page, not a catalog".** Use the direct file address. On GitHub, use the [Raw](#catalogs-on-github) one.
- **"ShadowMountPlus is not running".** Start ShadowMountPlus, for example through your autoloader.
- **PS4 games or games installed from PKG files are missing in My games.** Update ShadowMountPlus to 1.7beta4 or newer; older versions only list the games they mount.
- **A game shows "CAPTCHA / browser".** Choose **Solve CAPTCHA → Open on PS5**. This is the default for every host; a configured account or a different mirror does not bypass that menu.
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
