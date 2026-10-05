<p align="center"><img src="docs/icon.png" width="128" alt="PS5 Library"></p>
<h1 align="center">PS5 Library</h1>
<p align="center"><b>A game library for jailbroken PS5.</b><br>Pick a game, press <b>Download&nbsp;&amp;&nbsp;Install</b>, play it from your home screen.</p>

<p align="center"><img src="docs/store.jpg" alt="Store"></p>

## Features

- **One payload.** `ps5-library.elf` adds a **PS5 Library** tile to your home screen. The tile opens the library in the PS5 browser.
- **Feels like the console.** The game you select fills the screen, rows of games sit underneath, and every game has a full-screen page.
- **One button.** The console downloads, unpacks and installs the game. When it's done, the game is on your home screen.
- **Internal SSD or USB.** Pick a drive per game, or set a default. Games in exFAT/FFPKG format run from that drive through ShadowMountPlus. PKG games go through the system installer.
- **Downloads resume** after a pause, a dropped connection or a console restart.
- **Made for the controller.** Move with the D-pad, select with Cross, go back with Circle.
- **Covers and details:** size, region and minimum firmware for every game.
- **Any browser.** Open `http://<ps5-ip>:9999` on your phone or PC to queue games from the couch.
- **Optional accounts.** Some file hosts ask for a CAPTCHA. A premium key or a TorBox account downloads from them directly.

## Requirements

- A jailbroken PS5 with an ELF loader. Tested on firmware 13.60.
- [kstuff](https://github.com/EchoStretch/kstuff-lite)
- [ShadowMountPlus](https://github.com/drakmor/shadowMountPlus)

## Install

1. Download `ps5-library.elf` from [Releases](../../releases/latest).
2. Load it with your ELF loader (port 9021) or your payload manager. Add it to **autoload**: the home screen tile only works while the payload is running.
3. A notification says **PS5 Library added to the home screen**. Open the tile.
4. On first start, connect a catalog: enter its address and access key.

## Catalog

PS5 Library does not include, host or link to any games. It shows whatever catalog you connect. A catalog is an HTTP(S) address that returns JSON:

```json
{
  "entries": [
    {
      "id": "unique-id",
      "title": "Game name (v01.000)",
      "title_id": "PPSA01234",
      "kind": "game",
      "format": "exfat",
      "size": "52GB",
      "region": "EUR",
      "firmware": "4.xx",
      "source_sets": [
        {"host": "example.com", "files": [{"url": "https://example.com/file", "name": "PPSA01234.7z"}]}
      ]
    }
  ]
}
```

- `kind` is `game`, `dlc` or `update`.
- `format` is `exfat`, `ffpkg`, `ffpfs`, `ffpfsc`, `fpkg` or `pkg`.
- `source_sets` lists alternative mirrors. If one fails, the next one is tried.
- The access key is sent as `Authorization: Bearer <key>`.

## Screenshots

![Game page](docs/game.jpg)

| Browse | Downloads |
| --- | --- |
| ![Browse](docs/browse.jpg) | ![Downloads](docs/downloads.jpg) |

## Troubleshooting

- **The tile opens an empty page.** The payload isn't running. Load it again, or add it to autoload.
- **"ShadowMountPlus is not running".** Start ShadowMountPlus, for example through your autoloader.
- **A game shows "Needs an account".** All its mirrors are CAPTCHA hosts. Add a TorBox account or a premium key in **Settings → Download accounts**.

## Credits

Made by **deox**.

Shout-outs:
- **Pippo26442999**
- **[ps5upload](https://github.com/phantomptr/ps5upload)** by phantomptr. PS5 Library's package installer is built on it.
- [ShadowMountPlus](https://github.com/drakmor/shadowMountPlus) by drakmor
- [kstuff](https://github.com/EchoStretch/kstuff-lite)
- [ps5-payload-sdk](https://github.com/ps5-payload-dev/sdk) by John Törnblom
- [cpp-httplib](https://github.com/yhirose/cpp-httplib), [nlohmann/json](https://github.com/nlohmann/json), curl, libarchive, OpenSSL

## Disclaimer

PS5 Library is not affiliated with Sony Interactive Entertainment. It does not provide any games. Only install content you have the right to use.

## License

GPL-3.0-or-later, see [LICENSE](LICENSE). The source code will be published in this repository.
